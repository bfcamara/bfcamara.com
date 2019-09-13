---
layout: post
template: post
date: 2013-01-28
tags:
- cloud
- software development
title: "WebHooks testing tools: Requestb.in and LocalTunnel"
slug: /post/41699252979/webhooks-testing-tools-requestbin-and
description: "WebHooks testing tools: Requestb.in and LocalTunnel"
---
<div>
<div>
<div>
<p><strong>The goal of this post is to show how we can use the tools <a href="http://requestb.in/" title="Requestb.in - Inspect HTTP requests" target="_blank">Requestb.in</a> and <a href="http://progrium.com/localtunnel/" title="localtunnel - share localhost to the world" target="_blank">LocalTunnel</a> to make it easier the process of testing WebHooks integration.</strong></p>
</div>
<div></div>
</div>
</div>
<div>
<p>WebHooks offer an easy and elegant way for applications and services to integrate with one another. There is a trend that we have been watching in the last years, which consists in having an interconnected set of loosely coupled cloud services, talking to each other via HTTP. <strong>The term "webhooks" was introduced by <a href="http://progrium.com" title="Jeff Lindsay web site" target="_blank">Jeff Lindsay</a> in 2006, as a pattern in web architecture.</strong> Both services, RequestBin and LocalTunnel were created by Jeff.</p>
</div>
<div></div>
<div>
<p>Today there are a lot of cloud services which already offer WebHook integration, such as <a href="https://zapier.com/zapbook/webhook" title="Zapier webhooks integration" target="_blank">Zapier</a>, <a href="http://developer.uservoice.com/docs/service-hooks/introduction/)" title="UserVoice - WebHooks Integration" target="_blank">UserVoice</a>, <a href="http://webhooks.wordpress.com/2009/01/22/zendesk-targets-are-web-hooks/" title="ZernDesk - webhooks integration" target="_blank">ZenDesk</a>, <a href="https://help.github.com/articles/post-receive-hooks" title="GitHub - webhooks integration" target="_blank">GitHub</a>, etc.</p>
</div>
<div></div>
<div>
<p>The idea is quite simple. Taking the UserVoice as an example, when a new Ticket is posted on my UserVoice, I want to trigger an HTTP call to my CRM system to create a case and record the customer interaction on it. This is a typical use case of a webhook integration.&nbsp;</p>
</div>
<div></div>
<div>
<p><span>Here is the screen of user voice where I am defining the WebHook integration</span></p>
</div>
<div><span>&nbsp;</span></div>
<div><span><span><figure class="tmblr-full" data-orig-height="448" data-orig-width="500"><img alt="image" src="https://66.media.tumblr.com/807997727593365145d09169d7e0b0d7/30b06b4f6185de41-4b/s540x810/c13ec5933124b26f1034c1ca535ef1856cc843fa.png" data-orig-height="448" data-orig-width="500"></figure></span></span>
<p></p>
</div>
<div>
<p><span>&nbsp;</span><span>A WebHook is just an HTTP callback sent to a URL, defined by the user in response to an event.</span></p>
</div>
<div>
<div></div>
<div>
<p><strong>When I started to develop this kind of webhook integrations I felt the need to have a quick way to have real examples of the POST messages. The first Cloud service that I used to have this reals messages is Requestb.in.</strong></p>
</div>
<div></div>
<div>
<p>RequestBin lets me create a URL that will collect requests made to it, for further inspection in a human-friendly way. With RequestBin you can look at webhook requests. Here as an example of a request made in response to a new suggestion on the UserVoice site.</p>
</div>
<div></div>
<div><figure class="tmblr-full" data-orig-height="368" data-orig-width="500"><img alt="image" src="https://66.media.tumblr.com/a600d6f301ab19507130b95cbe24cdd7/30b06b4f6185de41-25/s540x810/dab0ac99c4f570082dc20ddb24a90bee5d8e8e8e.png" data-orig-height="368" data-orig-width="500"></figure><p></p>
<br><div>
<p>With Requestb.in you have a quick way to inspect the HTTP request that you have to handle.&nbsp;<span><strong>But what I really like is to have a way to test the WebHook in my local machine, which means that I have to expose my web server to the world</strong>, which in some scenarios it's not easy (firewall, NATs, etc.). <strong>We can use an easy way with <a href="http://progrium.com/localtunnel/" title="LocalTunnel" target="_blank">LocalTunnel</a> to solve this problem.</strong>&nbsp;</span></p>
</div>
<div></div>
<div>
<p>To use localtunnel, I need to install it in my local machine. Following the instructions on site, I rapidly noticed that I need Ruby and RubyGems, which I don't have it installed. Alternatively, and if I use version 2, I need to have Python, since the client is written in Python.</p>
</div>
<div></div>
<div>
<p>You can just skip this installation and use localtunnel for windows, which is written in C#, and <a href="https://github.com/danielrmz/localtunnel-net-client" title=".Net client for LocalTunnel service" target="_blank">available on GitHub</a>. That's what I did. &nbsp;In a further post I will show the how to use LocalTunnel on IIS Express.</p>
</div>
<div></div>
<div></div>
</div>
<div></div>
<div></div>
<div></div>
</div>
<div></div>