---
layout: post
template: post
date: 2014-02-18
tags:
- webhook
- rest
- api
- test
title: "API's testing tools"
slug: /post/77074334893/apis-testing-tools
description: "API's testing tools"
---
<p>I've been doing recently a lot of work integrating different backend systems, using its APIs, tipically in some form of a REST API. <strong>The current post has the main goal to enumerate tools that I currently use to become my work easier to explore and diagnose APIs, before getting my hands dirty with code.</strong></p>
<p><strong><a href="http://requestb.in/" target="_blank">RequestBin</a></strong></p>
<p>I've already introduced this tool in <a href="http://www.bfcamara.com/post/41699252979/webhooks-testing-tools-requestb-in-and-localtunnel" target="_blank">another post</a>. Basically, this tool allows us to check the requests that are being made by our application to a given endpoint.</p>
<p><figure class="tmblr-full" data-orig-height="395" data-orig-width="500" data-orig-src="./dd7109b6bbf55be3053b2b6d31363d25b29639fe7040f6032da00791159122a3.png"><img src="./339204d713fedee50c4a7b68251efbbf00f93e50be41b5e5d21cc68a463d0720.png" data-orig-height="395" data-orig-width="500" data-orig-src="./dd7109b6bbf55be3053b2b6d31363d25b29639fe7040f6032da00791159122a3.png"></figure></p>
<p></p>
<p></p>
<p><strong><a href="http://www.getpostman.com/" target="_blank">Postman</a></strong></p>
<p>Postman is a REST client that runs as a Chrome packaged app. <strong>It's an absolutely essential tool to use when you start exploring an API's service provider</strong>. Here are the main features:</p>
<ul><li><strong>Create requests quickly</strong>:
<ul><li>supports different formats (json, xml, url parameters, etc.)</li>
<li>suports basic authentication</li>
<li>provides oauth helpers</li>
<li>key/value editors for adding parameters or header values (works for URL parameters too)</li>
</ul></li>
<li>Document and share APIs:
<ul><li>Use collections to organize requests and share them quickly using URLs.</li>
<li>Document requests inside collections</li>
</ul></li>
<li>Collections:
<ul><li>Collections can be any group of requests.</li>
<li><strong>You can save any kind of request. While saving the request you can also add a name and a note</strong>.</li>
<li>Collections can be downloaded and shared as a file.</li>
</ul></li>
</ul><p><figure class="tmblr-full" data-orig-height="475" data-orig-width="500" data-orig-src="./e5c7bf8503249f18a812b6bb0a5f366be5eb8674e763048a0388887528a621aa.png"><img src="./434904c8d2b52ad7718bebd9b81548f85e31a16b93dc474be5dd55d23b29c3ba.png" data-orig-height="475" data-orig-width="500" data-orig-src="./e5c7bf8503249f18a812b6bb0a5f366be5eb8674e763048a0388887528a621aa.png"></figure></p>
<p></p>
<p><strong><a href="https://www.runscope.com" target="_blank">Runscope</a></strong></p>
<p>Runscope is the company that brings us RequestBin, and it has developed a SaaS app which allows us to test our APIs. It has different components:</p>
<ul><li>Runscope Radar: a test runner and framework</li>
<li><strong>API traffic inspector: allows to check all the traffic that is flowing through our account</strong></li>
<li><strong>Passageway: Passageway allows you and your team to temporarily share your local web server with others via a public URL</strong></li>
</ul><p>For the current work that I'm doing, I've just used API traffic inspector and Passageway</p>
<p><a href="https://www.runscope.com/docs/debugging" target="_blank">API traffic inpector</a></p>
<p>It's similiar to Requestbin, <strong>since it allows us to capture a given request, but it's more powerfull</strong>:</p>
<ul><li>it can act as a gateway to a given real endpoint</li>
<li>it can replay a previous response</li>
</ul><p><figure class="tmblr-full" data-orig-height="383" data-orig-width="500" data-orig-src="./2734dc421d21a1230e24bb90dd565ee67ec606379fa15bcc460c27856e61f62f.png"><img src="./76003d9c943386c1d2eafbbb2c5b90cbd09b81e7435a8903df456189a1c648f8.png" data-orig-height="383" data-orig-width="500" data-orig-src="./2734dc421d21a1230e24bb90dd565ee67ec606379fa15bcc460c27856e61f62f.png"></figure></p>
<p></p>
<p><a href="https://www.runscope.com/docs/passageway" target="_blank">Passageway</a></p>
<p><strong>It was never been easy to expose our local web server to the world.&nbsp;</strong>Just download the app and run it.</p>
<p><strong><a href="https://zapier.com/" target="_blank">Zapier</a></strong></p>
<p>Finally I want to say someting about Zapier. Zapier makes it easy to automate tasks between web apps. Sometimes I have to use Zapier to allow me to test an end to end scenario. Let me give you an example: the system I'm working on provides me an abstraction (a class) that allows me to call a web hook. Internally, the framework that uses that class does not allow to use HTTP calls, and requires that any call would be HTTPS. I was using requestbin to check the payload that the system was producing, but Requestbin does not allow to have HTTPS endpoints (at least without the SSL certificate warning). By that time I didn't know Runscope. So, my solution was to create a Zap in Zapier, that receives on a webhook listening an HTTPS endpoint and sends the request to another webhook on RequestBin. <strong>I strongly recommend to check Zapier, since it opens a variety of integration scenarios out of the box.</strong></p>
<p><strong><figure class="tmblr-full" data-orig-height="238" data-orig-width="500" data-orig-src="./8976714baa0f3ca4620277de4c8f02034a6113fd5310fdbba17cd6895826669e.png"><img src="./96a6677992c99cfd170d39f3bf93f71bdf3d607d64cd3bbeb6ab64035fd362b2.png" data-orig-height="238" data-orig-width="500" data-orig-src="./8976714baa0f3ca4620277de4c8f02034a6113fd5310fdbba17cd6895826669e.png"></figure></strong></p>
<p></p>
<p></p>
<p><strong>Conclusion</strong>: I'm sure there are other great tools out there, but these are the ones that I'm currently using. Besides the tools I've mentioned,. we should have a good understanding of REST architecture style, and how HTTP protocol works.</p>