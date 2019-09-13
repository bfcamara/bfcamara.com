---
layout: post
template: post
date: 2014-10-07
tags:
- vsonline
- REST
- oauth
title: "Consuming VS Online REST API using a Service Account "
slug: /post/99409744598/consuming-vs-online-rest-api-using-a-service
description: "Consuming VS Online REST API using a Service Account"
---
<p>One of the great things about the <a href="http://www.visualstudio.com/en-us/integrate/reference/reference-vso-overview-vsi.aspx">VS Online REST API</a> is that you no longer need to have installed Visual Studio or Team Explorer, or just the <a href="http://visualstudiogallery.msdn.microsoft.com/3278bfa7-64a7-4a75-b0da-ec4ccb8d21b6">TFS Object Model</a> to integrate with TFS. It's just a normal REST API we can consume by making HTTP calls.</p>
<p>Following the <a href="http://www.visualstudio.com/integrate/get-started/get-started-rest-basics-vsi">documentation on VS Online site</a>, it seems that <strong>we are limited to consume the REST API using alternate credentials (basic&nbsp;</strong><strong>authentication with username and password) or using OAuth (bearer access tokens).</strong></p>
<p><span>This is not perfect if we're developing some integration work in VS Online and we want to use a Service Account to perform some tasks such as&nbsp;</span>queue a build, create work items, etc.</p>
<p>Well, if it's possible to use service accounts when using TFS Object Model, there should be a way to use them as well with VSO REST API . In fact <strong>there is a way to use a service account in the REST API</strong>, and this post aims to show you how to do it.</p>
<p>First and foremost, we need to get a Service Account in VS Online. For that we can use the tool <a href="http://nakedalm.com/tfs-service-credential-viewer/">TFS Service Credential Viewer</a>&nbsp;provided&nbsp;by&nbsp;<a href="http://nakedalm.com/">Martin Hinshelwood from naked ALM</a>.&nbsp;You can even <a href="http://youtu.be/Fkn6V0_zz28">watch a video</a> explaining how to do it.</p>
<p>Next, in order to use these credentials with REST API, we need to understand what TFS Object Model does and try to replicate the same behavior in REST API and check if we succeed. Basically TFS OM uses <a href="http://msdn.microsoft.com/en-us/library/azure/hh674475.aspx">OAuth WRAP Protocol to get an access token from Azure Access Control Service &nbsp;(ACS)</a>.</p>
<p>With the help of <a href="www.telerik.com/fiddler">Fiddler</a>, here are the interactions we got when using TFS OM:</p>
<ol><li><span>Hit VS Online unauthenticated to get a redirect 302 response where there are <strong>some headers that contain federation data</strong>, namely,&nbsp;</span>realm and issuer</li>
<li><strong>Request a token from ACS</strong> via the OAuth WRAP Protocol passing the Service Account username and password</li>
<li><strong>Use the returned token in the Authorization header with a WRAP scheme</strong> when invoking TFS services.</li>
</ol><p></p>
<p>Let's try to code this in C# and see what we got.&nbsp;If you want a full gist we can grab it from <a href="https://gist.github.com/bfcamara/6aab0858ebda2db435a9" target="_blank">here</a>.</p>
<p>Let's start with Step 1 to get federation data.</p>
<p><figure class="tmblr-full" data-orig-height="265" data-orig-width="500" data-orig-src="./a8b7ab62b2e09e94ba0ad35cb9f16cdf202f7f01e634eac428e5b4285aa6e27c.png"><img alt="image" src="./9fe7e420702dcbc3b2d6457a633aec92ee701d634fdac3d765dcebc25d613ed9.png" data-orig-height="265" data-orig-width="500" data-orig-src="./a8b7ab62b2e09e94ba0ad35cb9f16cdf202f7f01e634eac428e5b4285aa6e27c.png"></figure></p>
<p>Having the federation Realm and Issuer, we can now request an access token</p>
<p><figure class="tmblr-full" data-orig-height="225" data-orig-width="500" data-orig-src="./eb84095afeaa1fbd6cdcb9480489a3d20fd9959dadb81388f64772934b5bb1a8.png"><img alt="image" src="./950b70b80760feef63ad958cfb36e588aad66ba164ba99f5257eb1c4fc1fef22.png" data-orig-height="225" data-orig-width="500" data-orig-src="./eb84095afeaa1fbd6cdcb9480489a3d20fd9959dadb81388f64772934b5bb1a8.png"></figure></p>
<p>And finally we can use the access token to consumer our REST API, to get the list of team projects for example.</p>
<p><figure class="tmblr-full" data-orig-height="273" data-orig-width="500" data-orig-src="./e8175553fd9e52285b01981255f5cd0fd8d1c359c3f777676d44c7160c221bd6.png"><img alt="image" src="./6178531cbfe1a93155a66506aa452ff8f1d66b83e3c663a96ff79d226a1ac841.png" data-orig-height="273" data-orig-width="500" data-orig-src="./e8175553fd9e52285b01981255f5cd0fd8d1c359c3f777676d44c7160c221bd6.png"></figure></p>
<p></p>
<p></p>
<p>Here is the result for my account</p>
<p><figure class="tmblr-full" data-orig-height="634" data-orig-width="419" data-orig-src="./78dcaa0a885aa1fce893b588be36a31eee137d185ea43dcf37feddfc1bbd457a.png"><img alt="image" src="./1af137c20a21e75c0f466c9bf57e6cca96646ea186618e1ee12542d75ed98c47.png" data-orig-height="634" data-orig-width="419" data-orig-src="./78dcaa0a885aa1fce893b588be36a31eee137d185ea43dcf37feddfc1bbd457a.png"></figure></p>
<p></p>
<p>It works!</p>
<p>As a side note,<strong> this method of consuming the REST API is not documented in VS Online site which means that we have no guarantees that&nbsp;it will still working in the future</strong>.</p>
<p>&nbsp;I hope it helps.</p>
<p></p>