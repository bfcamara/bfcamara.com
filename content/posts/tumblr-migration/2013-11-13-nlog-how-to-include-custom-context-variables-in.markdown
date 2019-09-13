---
layout: post
template: post
date: 2013-11-13
tags:
- logging
- .net
- asp.net
title: "NLog: How to include custom context variables in all your logs"
slug: /post/66886024762/nlog-how-to-include-custom-context-variables-in
description: "NLog: How to include custom context variables in all your logs"
---
<p><strong>Logging is a very important aspect of every application</strong>. There are a lot of logging frameworks available for the .Net platform, and I won't discuss which one is better. Many times it is a matter of taste. For example, if we are .Net developers but develop for Java as well, it makes sense to choose <a href="http://logging.apache.org/log4net/" target="_blank">Log4Net</a> since it is port of the well known <a href="http://logging.apache.org/log4" target="_blank">Log4j</a>, so you can capitalize your knowlodge in both platforms.</p>
<p><strong>In the past I've used a lot Log4Net, but nowadays, my logging framework of choice is <a href="http://nlog-project.org/" target="_blank">NLog</a></strong>. For the vast majority of scenarios there is no big difference between platforms, but since NLog was written entirely with .Net in mind, I tend to prefer NLog. Both platforms have great community support and there are a lot of configuration choices available for different scenarios (file logging, sql logging, etc.)</p>
<p>In this post I would like to show you a not very well known feature, in which <strong>we can include some&nbsp;custom context variables in our log statements, without the need to specify the variables every time we are emiting the log</strong>.</p>
<p><strong>Here is the scenario: we are developing a SaaS web application, and we want to include in all our logs the subscription id, for all web requests</strong>.The subscription id identities univocally the subscription that is issuing the web request.</p>
<p></p>
<p><strong><u>Introducing Mapped Diagnostics Context</u></strong></p>
<p>From the <a href="http://nlog-project.org/documentation/v2.0.1/html/T_NLog_MappedDiagnosticsContext.htm" target="_blank">NLog documentation</a></p>
<blockquote>
<p><em>Mapped Diagnostics Context is a thread local structure that keeps a dictionary of strings and provides methods to output them in layouts</em></p>
</blockquote>
<p>Here is the code we have write to add a key to this dictionary</p>

```cs
NLog.MappedDiagnosticsContext.Set("subscriptionid", subscriptionId.ToString());
```

<p>After running this line of code, the key subscriptionid is available in the thread local structure. We now need to configure our logging layout to include this key in our logs. Here is a typical configuration</p>
<p><figure class="tmblr-full" data-orig-height="227" data-orig-width="500"><img alt="image" src="https://66.media.tumblr.com/3fa6e7fd3cc826c171148d73764b7bc0/b3956245fd2a64c0-e8/s540x810/1be897f8d8a39205d355fbad7a15b6b6a39a7b25.png" data-orig-height="227" data-orig-width="500"></figure></p>
<p></p>
<p>To include the subscription id in the logs we've just used <strong>${mdc:item=subscriptionid}</strong></p>
<p>We can now to grep/find all logs for a given "suspicious" subscription. Let me check some suspicious activity on subscription 25</p>
<pre>grep "[S:25]" *.log 
</pre>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p><strong><u>When to set Mapped Diagnostics Context</u></strong></p>
<p>We wish to set our mapped diagnostics context keys as soon as possible. For example, if your application is an Asp.Net MVC application, you can set at the beginning of your action, or as an alternative, in an action filter.</p>
<p></p>
<p><strong><u>Is this works with async/await?</u></strong></p>
<p>One thing to notice about <strong>MappedDiagnosticsContext is that is a dictionary stored in thread local storage (TLS)</strong>. This is very important to perceive, in particular if you are in a multi-thread environment. Even <strong>if you are using the new paradigm of async and await for asynchronous programing, which is awesome and makes extremely easy for the developer to program in asynchronous way, we have to understand that the TLS does not flow in async points</strong>. This means that we will loose this information as soon as we get another thread different from the one where we set the value.</p>
<p>So, how can we make this works with async/await, and guarante that we have always our subscription id in our logs, even if&nbsp;a web request is served by multiple threads during the lifecycle of the request? Basically, <strong>we will have to use a different storage from the TLS, one that flows in async poinst. We can use the CallContext. In my next post I will show you how to implement this feature</strong>.</p>
<p>Btw, if you are using Log4net, it is already supported out of the box with <a href="http://logging.apache.org/log4net/release/manual/contexts.html" target="_blank">contexts</a>.</p>
<p>Log4Net supports different scope contexts:</p>
<ul><li>Global (log4net.GlobalContext) - The global context is shared by all threads in the current AppDomain. This context is thread safe for use by multiple threads concurrently. The NLog equivalent is the <strong>NLog.GlobalDiagnosticsContext</strong></li>
<li>Thread (log4net.ThreadContext) - The thread context is visible only to the current managed thread. The NLog equivalent is the <strong>NLog.MappedDiagnosticsContext.</strong></li>
<li>LogicalTread (log4net.ThreadLogicalContext). The logical thread context is visible to a logical thread. Logical threads can jump from one managed thread to another. For more details see the .NET API System.Runtime.Remoting.Messaging.CallContext.</li>
</ul><p>I haven't figured the equivalent in NLog to the log4net.ThreadLogicalContext. So I had to implement it in my current project. Wait for the next post.</p>