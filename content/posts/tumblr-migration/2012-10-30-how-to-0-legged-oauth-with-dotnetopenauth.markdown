---
layout: post
template: post
date: 2012-10-30
tags:
- oauth
- dotnetopenauth
title: "How-to: 0-legged OAuth with DotNetOpenAuth"
slug: /post/34630752066/how-to-0-legged-oauth-with-dotnetopenauth
description: "How-to: 0-legged OAuth with DotNetOpenAuth"
---
<p></p><div>As promissed in <a href="http://bfcamara.com/post/34158128493/oauth-2-legged-vs-0-legged-uservoice-api-as-an" title="2-legged vs 0-legged Oauth" target="_blank">my previous post</a>, where I dived in the the difference between a 2-legged and a 0-lgeed oauth, <strong>I will show in this post how to build a 0-legged oauth using the DotNetOpenAuth library</strong>. Just to remember, a 0-legged oauth consists of a consumer accessing the protected resources submitting oauth signed requests using just the consumer key and the consumer secret.</div>
<div><span><br></span></div>
<div><span>I will take as an example the UserVoice API, the method to list all the users for a given site, which requires a 0-legged API.</span></div>
<div><span><br></span></div>
<div><span>Before you use the UserVoice API or another oauth API from a service provider, <strong>the first thing to to is to get a consumer key and a consumer secret</strong>. This process is more or less the same in all service providers, but you have to check with the service provider how to get them. Here is a screenshot how I got this data from UserVoice site, in the admin console</span></div>
<div><span><br></span></div>
<div><span><figure class="tmblr-full" data-orig-height="128" data-orig-width="500"><img src="https://66.media.tumblr.com/7511578c75e07d5fb945398b686f7c3a/50a65bbf97ea6c28-fb/s540x810/d52f917bd638cf7f9339474e98844f22ba9014fe.jpg" data-orig-height="128" data-orig-width="500"></figure></span></div>
<div><span><br></span></div>
<div><span><br></span></div>
<div><span>Next, in our visual studio project, we have to get the libraries of dotnetopenauth. Since we are consuming an oauth API, <strong>we have to get the Nuget package DotNetOpenAuth.OAuth.Consumer</strong>. </span></div>
<div><span><br></span></div>
<div><span><br></span></div>
<div><span><figure class="tmblr-full" data-orig-height="332" data-orig-width="500"><img src="https://66.media.tumblr.com/40ab5d3ccd5b573684e291174d23aaf8/50a65bbf97ea6c28-38/s540x810/54ba7846f7aae86bf79981cc628610b1d07541df.jpg" data-orig-height="332" data-orig-width="500"></figure></span></div>
<div><span><br></span></div>
<div><span><strong>The main class when consuming an API is the class WebConsumer in namespace DotNetOpenAuth.OAuth</strong>. The constructor of this class receives two arguments as show below</span></div>
<div><span><br></span></div>
<div>

```cs
public WebConsumer(ServiceProviderDescription serviceDescription, IConsumerTokenManager tokenManager);
```

</div>
<div><span><strong><br></strong></span></div>
<div><span><strong>The serviceDescription defines the endpoints and the behavior of the service provider</strong>. <strong>The token manager is the component responsible to manage the tokens used by a web site in its role as a consumer</strong>.</span></div>
<div><span><br></span></div>
<div>
<div><span>In our case the serviceDescription is something like this</span></div>
<div><span><br></span></div>
<div>

```cs
var providerDesc = new ServiceProviderDescription()
{
	RequestTokenEndpoint = new MessageReceivingEndpoint("https://bfcamara.uservoice.com/oauth/request_token", HttpDeliveryMethods.PostRequest),
	AccessTokenEndpoint = new MessageReceivingEndpoint("https://bfcamara.uservoice.com/oauth/access_token", HttpDeliveryMethods.PostRequest),
    UserAuthorizationEndpoint = new MessageReceivingEndpoint("https://bfcamara.uservoice.com/oauth/authorize", HttpDeliveryMethods.PostRequest),
    ProtocolVersion = ProtocolVersion.V10a,
    TamperProtectionElements = new ITamperProtectionChannelBindingElement[] {
		new HmacSha1SigningBindingElement()
	}
};
```
</div>
<div><span><br></span></div>
<div><span>Basically we are describing the oauth endpoints that will be used in the oauth dance, the oauth version, and the signing policy used in the service provider, which in this case is &nbsp;the method HMAC-SHA1 defined in RFC2104.&nbsp;</span><strong>Next we have to write our token manager, which must implement the interface IConsumerTokenManager (whic must implement the ITokenManager interface)</strong>. Using the Alt-F10 Implement Interface of Visual Studio we get the skeleton of our class</div>
<div><span><br></span></div>
<div>

```cs
public class ZeroLeggedTokenManager : IConsumerTokenManager
{
    public string ConsumerKey
    {
        get { throw new NotImplementedException(); }
    }

    public string ConsumerSecret
    {
        get { throw new NotImplementedException(); }
    }

    public void ExpireRequestTokenAndStoreNewAccessToken(string consumerKey, string requestToken, string accessToken, string accessTokenSecret)
    {
        throw new NotImplementedException();
    }

    public string GetTokenSecret(string token)
    {
        throw new NotImplementedException();
    }

    public TokenType GetTokenType(string token)
    {
        throw new NotImplementedException();
    }

    public void StoreNewRequestToken(DotNetOpenAuth.OAuth.Messages.UnauthorizedTokenRequest request, DotNetOpenAuth.OAuth.Messages.ITokenSecretContainingMessage response)
    {
        throw new NotImplementedException();
    }
}
```

</div>
<div></div>
<div></div>
<div>Since we are using a 0-legged oauth, we have just to worry with the GetTokenSecret method, and the getter properties of our consumer key and consumer secret. Here is the final version of our token manager</div>
<div><span><br></span></div>
<div>

```cs
public class ZeroLeggedTokenManager : IConsumerTokenManager
{
    private string _consumerKey;
    private string _consumerSecret;

    public ZeroLeggedTokenManager(string consumerKey, string consumerSecret)
    {
        _consumerKey = consumerKey;
        _consumerSecret = consumerSecret;
    }

    public string ConsumerKey { get { return _consumerKey; }}

    public string ConsumerSecret { get { return _consumerSecret; } }

    public void ExpireRequestTokenAndStoreNewAccessToken(string consumerKey, string requestToken, string accessToken, string accessTokenSecret)
    {
        throw new NotImplementedException();
    }

    public string GetTokenSecret(string token)
    {
        //In a 0-legged conversation only the consumer secret is used to sign the message
        return "";
    }

    public TokenType GetTokenType(string token)
    {
        throw new NotImplementedException();
    }

    public void StoreNewRequestToken(DotNetOpenAuth.OAuth.Messages.UnauthorizedTokenRequest request, DotNetOpenAuth.OAuth.Messages.ITokenSecretContainingMessage response)
    {
        throw new NotImplementedException();
    }
}
```

</div>
<div><span><br></span></div>
<div><span>Now we can create our token manager</span></div>
<div><span><br></span></div>
<div>

```cs
var tokenManager = new ZeroLeggedTokenManager(CONSUMER_KEY, CONSUMER_SECRET);
```


</div>
<div><span><br></span></div>
<div><span>The CONSUMER_KEY and CONSUMER_SECRET are constants which contains the values of the consumer key and secret&nbsp;respectively.&nbsp;<br></span></div>
<div><span><strong><br></strong></span></div>
<div><span><strong>Finally we can create our consumer and use it to make the call, using the method PrepareAuthorizeRequestAndSend</strong></span></div>
<div><span><br></span></div>
<div>

```cs
var zeroLeggedWebConsumer = new DotNetOpenAuth.OAuth.WebConsumer(providerDesc, tokenManager);

var response = zeroLeggedWebConsumer.PrepareAuthorizedRequestAndSend(
    new MessageReceivingEndpoint("https://bfcamara.uservoice.com/api/v1/users.json",
       HttpDeliveryMethods.GetRequest), "DUMMY");
```


</div>
<div><span><br></span></div>
<div>When calling the PrepareAuthorizedRequestToken, we have to pass the endpoint to call, and the token to use, which will be used by the consumer to ask to our token manager what is the secret associated with this token. <strong>There is a little trick here: since our secret is always an empty secret returned by the token manager, it's quite irrelevant what you pass to the method, however it cannot be null or empty</strong>, which is not allowed. In our case we pass "DUMMY" as the token. (If you pass an empty or null token you will get an exception at runtime thrown by dotnetopenauth library).</div>
<div><span><br></span></div>
<div><span><br></span></div>
<div><span>Using Fiddler Web Debugger, you can check the content of the request/response</span></div>
<div><span><br></span></div>
<div><span><br></span></div>
<div><figure class="tmblr-full" data-orig-height="346" data-orig-width="500"><img src="https://66.media.tumblr.com/6253cec7b773c3950bedd6e7368000bd/50a65bbf97ea6c28-00/s540x810/50b0440b53be969f5c7c5b59143158858299e2be.jpg" data-orig-height="346" data-orig-width="500"></figure></div>
<div><span><br></span></div>
<div><span><br></span></div>
<div>Now you can parse the response as you want.</div>
<div><span><br></span></div>
<div><strong>Sumarizing, in this post, I demonstrated how to use the DotNetOpenAuth library to consume API's with a 0-legged OAuth.</strong></div>
<div><span><br></span></div>
<div><span><br></span></div>
<div></div>
</div>