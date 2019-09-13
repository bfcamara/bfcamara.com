---
layout: post
template: post
date: 2013-12-04
tags:
- asp.net
- security
- membership
title: "ASP.NET Identity 1.0: where the hell is the recover password feature?"
slug: /post/68977237610/aspnet-identity-10-where-the-hell-is-the
description: "ASP.NET Identity 1.0: where the hell is the recover password feature?"
---
<p><strong>Answer</strong>: it's not available! Surprised!? Yes, I am very surprised!</p>
<p>&nbsp;</p>
<p>For many years we've used the <a href="http://msdn.microsoft.com/en-us/library/yh26yfzy(v=vs.98).aspx" target="_blank">ASP.NET Membership</a> (or the SimpleMembership included in Webpages). Here are the features included out-of-the box in the membership system:</p>
<ul><li>Authentication</li>
<li>User Registration</li>
<li>Roles Management</li>
<li>Password Management</li>
<li>Profile Management</li>
</ul><p>&nbsp;</p>
<p><strong>With .Net 4.5.1 we have a new identity management system called ASP.NET Identity</strong></p>
<p>. It's normal to compare the two systems, since they have the same main goal, which is to provide an identity management system.</p>
<p>&nbsp;</p>
<p>I don't want to go with the details of this new system, and the reasons that took Microsoft to write it. You can check a great introduction <a href="http://www.asp.net/identity/overview/getting-started/introduction-to-aspnet-identity" target="_blank">here</a>.</p>
<p>&nbsp;</p>
<p>If you decide to use this new system (as I did in my current project) to provide a basic user management system in you application, allowing a user to have an usual username/password credentials, <strong>you will be very surprised (and maybe disapointed) by the fact that the password recover feature it's not included out-of-the-box</strong>. Of course we can implement it by hand, but the truth is that the ancient Membership system has included this feature, so <strong>it seems obvious to think and expect that this new one would also include it</strong>. Wrong assumptions. So, if you check the UserManager class (the main class to manage identity features), you won't find any method to generate a reset password token to be used by the application user to be allowed to reset his password.</p>
<p>&nbsp;</p>
<p>After some research, <strong>it seems that the next version of the ASP.NET Identity, version 1.1, will include the recover password feature</strong>.</p>
<p>Since October 2013, Microsoft decided to make available the nightly builds of the ASP.NET Identity system. You can get this nightly builds using NuGet packages available on MyGet. Check <a href="http://blogs.msdn.com/b/webdev/archive/2013/10/09/asp-net-identity-nuget-packages-for-the-nightly-builds-are-available-on-myget.aspx" target="_blank">the steps to follow in Visual Studio to configure the MyGet feed to pull in the nightly packages for ASP.NET Identity</a></p>
<p><figure class="tmblr-full" data-orig-height="171" data-orig-width="500"><img alt="image" src="https://66.media.tumblr.com/7df31af9e25d5065d82ba5732adae314/1c64b843cc41f48c-7c/s540x810/cca0b4996a5fdac5d7c5fce70664e7a523f476c9.png" data-orig-height="171" data-orig-width="500"></figure></p>
<p></p>
<p><span>&nbsp;</span></p>
<p>What I will be showing next is based on the packages with version 1.1.0-alpha1-131108 that I grabbed in beginning of November, which is a pre-release version, and maybe the things have change since then.</p>
<p>&nbsp;</p>
<p>Checking again the UserManager class, we now have some methods/properties/interfaces related to the password recover feature</p>

```csharp    
public interface ITokenProvider
{
    string Generate(UserToken token);
    UserToken Validate(string token);
}

public class UserManager<TUser, TKey> : IDisposable
    where TUser : global::Microsoft.AspNet.Identity.IUser
    where TKey : global::System.IEquatable
{
    //...

    public ITokenProvider PasswordResetTokens { get; set; }
    public virtual Task GetPasswordResetTokenAsync(TKey userId);
    public virtual Task ResetPasswordAsync(TKey userId, string token, string newPassword);
    
    //...
}
    
```

<p>After taking some time with the disassembled code, what we need is quite simple. We need an implementation of ITokenProvider and set it as the PasswordResetTokens of the UserManager instance that we are using. I am doing this at the AccountController when initializing the UserManager. Here is an example</p>

```csharp
[RoutePrefix("api/accounts")]
public class AccountController : ApiControllerBase
{
    //...

        AccountController()
    {
        //...
        
        UserManager.PasswordResetTokens = new DataProtectorTokenProvider(Startup.ResetPasswordDataProtectorFactory());
    }
    
    //...
}
        
```

<p>After this we can:</p>
<ol><li>use the method GetPasswordResetTokenAsync to get the token to send by email to the user, allowing him to reset the password</li>
<li>validate the token using UserManager.PasswordresetTokens.Validate. In case of success we get an instance of the class UserToken which has the property UserId</li>
<li>and finally, we can use the ResetPasswordAsync to reset the password</li>
</ol><p>&nbsp;</p>
<p>So, basically we need an implementation of an ITokenProvider. We can use whatever we want to generate and validate the token, but we can't forget that we are in the middle of a security problem, so it's obvious that we have to be carefull with our implementation. In the example above, I am using the DataProtectorTokenProvider, available in the nuget Microsoft AspNet Identity Owin (the Owin implementation for ASP.NET identity. You can read more about Owin <a href="http://www.asp.net/vnext/overview/owin-and-katana" target="_blank">here</a>). This is a generic implementation, which receives an instance of IDataProtector (defined in Microsoft.Owin.Security package)</p>

```csharp
namespace Microsoft.Owin.Security.DataProtection
{
    // Summary:
    //     Service used to protect and unprotect data
    public interface IDataProtector
    {
        // Summary:
        //     Called to protect user data.
        //
        // Parameters:
        //   userData:
        //     The original data that must be protected
        //
        // Returns:
        //     A different byte array that may be unprotected or altered only by software
        //     that has access to the an identical IDataProtection service.
        byte[] Protect(byte[] userData);
        //
        // Summary:
        //     Called to unprotect user data
        //
        // Parameters:
        //   protectedData:
        //     The byte array returned by a call to Protect on an identical IDataProtection
        //     service.
        //
        // Returns:
        //     The byte array identical to the original userData passed to Protect.
        byte[] Unprotect(byte[] protectedData);
    }
}
```

<p>In the examle above, I am using the intrastructure provided by Owin Security, which uses <a href="http://msdn.microsoft.com/en-us/library/ms995355.aspx" target="_blank">Windows Data Protection API</a>. In the Owin Startup class, I define the data protector factory.</p>

```csharp
public partial class Startup
{
    //...
    internal static Func<IDataProtector> ResetPasswordDataProtectorFactory;

    public void ConfigureAuth(IAppBuilder app)
    {
        //...
    
        ResetPasswordDataProtectorFactory = () => app.CreateDataProtector("ResetPassword");
        
        //...

    }
}
```

<p>The app.CreateDataProtector uses a <a href="http://msdn.microsoft.com/en-us/library/system.security.cryptography.dpapidataprotector(v=vs.110).aspx" target="_blank">DpapiDataProtector</a>.</p>
<p><strong>As a conclusion</strong>, I was very surprised by the fact Microsoft decided to not include some features in ASP.NET Identity 1.0, such as password recovery. However, I am very glad to know in advance they are working on a new version 1.1 version, which apparently will include some of the features. For now, you can take the nightly builds to get the features (on your own risk) or implement it by hand.</p>