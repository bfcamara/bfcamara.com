---
layout: post
template: post
date: 2012-10-23
title: "OAuth: 2-legged vs 0-legged (UserVoice API as an example)"
slug: /post/34158128493/oauth-2-legged-vs-0-legged-uservoice-api-as-an
description: "OAuth: 2-legged vs 0-legged (UserVoice API as an example)"
---
<p><img align="right" alt="Oauth" height="250" src="./f188ae47e7ef202e0f7938896a595a3d28b3d97a940886d65cc1bc1406ba2c60.png" width="250" /></p>&#13;
<div>&#13;
<div><span><br /><br /></span></div>&#13;
<div><span>Nowadays it's very common to use <a href="http://oauth.net/" title="OAuth Community Site" target="_blank">OAuth </a>as the method for clients to access server resources on behalf of a resource owner. OAuth also provides a process for end-users to authorize 3rd-party access to their server resources without sharing their credentials (tipically, a username and password pair), using user-agent redirections.  Google, Twitter, Facebook, LinkedIn, and many other services providers, use OAuth in their APIs.</span></div>&#13;
<div><span><br /></span></div>&#13;
<div><span>However, <strong>there is some debate in the community about  the interpretation of the number of legs when using OAuth</strong>. The curious thing is that  both RFCs RFC-5849 for OAuth 1.0 and RFC-6749 for OAuth 2.0, does not use the term leg at all. So, it seems the term leg is kind of an interpretation of the spec, and of course there are multiples interpretations in the community. There are those who consider that the number of legs are the number of steps involved to get the access token. And there are those who consider that the number of legs are the number of parties involved in OAuth dance. <strong>For me, it makes more sense to consider that the number of legs is the number of steps involved to obtain to the access token, which allow me to distinct between a 2-legged from a 0-legged oauth</strong> (more on that later). </span></div>&#13;
<div><span><br /></span></div>&#13;
<div><span>The most used oauth dance uses a 3-leg process, and it seems that there is agreement about this 3-leg definition:</span></div>&#13;
<ol><li><span>The consumer (the app that wants to access data) obtains a request token from the service provider</span></li>&#13;
<li><span>The consumer redirects the user to the service provider , using the request token obtained. The user logs in  and grants the requested permissions on the service provider pages</span></li>&#13;
<li><span>The service provider redirects the user to the consumer site, and the consumer obtains the access token from the service provider</span></li>&#13;
</ol><div><span><strong>The division in the community is when refering to 2-legged OAuth (vs a 0-legged OAuth)</strong>. </span>In my point of view, <strong>the 0-legged OAuth consists of a consumer accessing the protected resources submitting OAuth signed requests using just the consumer key and the consumer secret</strong>. The process of obtaining these is defined by each service provider and is once per application, usually by the application developer. <strong>This is different from the 2-legged OAuth, which is a 3-legged OAuth without requiring the end-user to participate (missing step 2 and no redirections) .</strong> <a href="http://blog.nerdbank.net/" title="Andrew Arnott Blog" target="_blank">Andrew Arnott</a> has a great post where he explains  <a href="http://blog.nerdbank.net/2011/06/what-is-2-legged-oauth.html" title="What is 2-legged OAuth" target="_blank">What is 2-legged OAuth</a>.  If you want to have a sense of the discussion about the 2-legged OAuth vs 0-legged OAuth, see the comments of his post.</div>&#13;
<div><span></span></div>&#13;
<div><span><br /></span></div>&#13;
<div><span>I had to learn the distinction between the 2-legged OAuth and 0-legged OAuth when trying to integrate with UserVoice API. In the <a href="http://developer.uservoice.com/docs/api/getting-started/" title="UserVoice API Getting Started" target="_blank">Getting Started section of Uservoice API documentation</a>, it is said</span></div>&#13;
<div><span><br /></span></div>&#13;
<blockquote>&#13;
<div><span><em>There are three types of requests:<br /><br /> - Unauthenticated Requests- Requests you can make without any authentication other than passing your API Key. ex: Retrieving ideas or comments from a public forum.<br /> - 3-legged OAuth requests- Requests made on the behalf of a user (see any API endpoints marked as requiring a User or Admin). ex: Voting on an idea, Creating a comment, Responding to a idea..<br /> - 2-legged OAuth requests- Requests made on behalf of the client not on behalf of any specific user (see any API endpoints marked as requiring a Trusted Client) ex: Retrieving all users for a site, Getting a list of ideas for a private forum.</em></span></div>&#13;
</blockquote>&#13;
<div><span><br /></span></div>&#13;
<div><span><br /></span></div>&#13;
<div><span>In my case I was developing the method to retrieve all users for a site, which is refered to be a 2-legged OAuth request. I am using the  library <a href="http://www.dotnetopenauth.net/" title="DotNetOpenAuth Library" target="_blank">dotnetopenauth</a>, which it seems to me the most used oauth library in the .Net world.  </span></div>&#13;
<div><span><br /></span></div>&#13;
<div><span>In my point of view I had to get a request token first and then exchange it to get the access token, and finally use the access token when signing the requests to access resources. But when submitting the second request to get the access token I always got a 401 Unauthorized. </span></div>&#13;
<div><span><br /></span></div>&#13;
<div><span>I decided to get some help from the support, and here is the answer that I received by email</span></div>&#13;
<div><span><br /></span></div>&#13;
<blockquote>&#13;
<div><span><em>Hi Bruno,<br /><br /> Our Developers figured out what's going on.<br /><br /><strong>You're trying to do 2-legged OAuth basically like 3-legged OAuth without the user part. This is what most people mean by 2-legged OAuth. We don't support this type of 2-legged OAuth at this time.</strong><br /><br /><strong>What we call 2-legged OAuth in our docs is what some people have suggested calling 0-legged OAuth. It is basically signing requests with the client secret only.</strong><br /><br /> In our examples we do this by creating an AccessToken object for a user. At least in the Ruby OAuth library, this sets the token and secret both to empty string. We then use this token to sign the requests. There is no need to ask the API for a request token or for an access token. You can just create a blank access token and start making requests.<br /><br /> Let me know if you have any more questions.</em></span></div>&#13;
</blockquote>&#13;
<div><span> </span></div>&#13;
<div><span><br /></span></div>&#13;
<div><span>So what can we conclude from this? <strong>We have to understand from the service provider perspective what they are refering as a 2-legged oauth, since there are this 0-legged oauth which is different</strong>. In the next post I will show how to to this 0-legged oauth using dotnetopenauth, since there are some tricks to do it.</span></div>&#13;
<div><span><br /></span></div>&#13;
<div><span><br /></span></div>&#13;
<div><span> </span></div>&#13;
</div> 