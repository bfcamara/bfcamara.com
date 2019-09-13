---
layout: post
template: post
date: 2013-11-22
tags:
- nlog
- asp.net
- logging
title: "NLog - How to include custom context variables in all your logs, that will flow across over async points"
slug: /post/67752957760/nlog-how-to-include-custom-context-variables-in
description: "NLog - How to include custom context variables in all your logs, that will flow across over async points"
---
<b><figure class="tmblr-full" data-orig-height="78" data-orig-width="352" data-orig-src="./f0032c0313df8f4e90aa2d638c24bfb21b07990f4f99bb8364303bf4d2310678.png"><img src="./2f1a9498be5cb9500b94639d62a6166d897676d9c37b9e2992514676f561ad67.png" alt="image" data-orig-height="78" data-orig-width="352" data-orig-src="./f0032c0313df8f4e90aa2d638c24bfb21b07990f4f99bb8364303bf4d2310678.png"></figure></b><p></p>

<p><b>Update: The <a href="https://github.com/NLog/NLog/wiki/MDLC-Layout-Renderer">Mdlc </a>feature shown in this post is now part of NLog.</b></p><p><strike><b>TL;DR:</b><b>If you need to have custom context variables that will flow across async points, you can use the <a href="https://www.nuget.org/packages/NLog.Contrib/">NLog.Contrib nuget package</a> to have the MappedDiagnosticsLogicalContext class and the ${mdlc} layout render</b>.</strike></p>
<p>In my previous post, <a href="http://www.bfcamara.com/post/66886024762/nlog-how-to-include-custom-context-variables-in-all">NLog: How to include custom context variables in all your logs</a>, I presented the <b>NLog.MappedDiagnosticsContext class and how can it be used to emit custom context variables to all of your logs. This class keeps a dictionary of strings in the thread local storage (TLS)</b>. When I want to emit always a given context property, I have to add this line of code</p>

```cs
NLog.MappedDiagnosticsContext.Set("subscriptionid", SubscriptionId);
```

<p>In this case the SubscriptionId is the custom context property that I will be attaching to all my log statements. For example, if you are <b>in the context of Web API 2, you can set the property on a base controller</b>, just like this</p>

```cs
public override Task<HttpResponseMessage> ExecuteAsync(HttpControllerContext controllerContext, CancellationToken cancellationToken)
{
    if (User != null && User.Identity != null && User.Identity.IsAuthenticated)
        NLog.MappedDiagnosticsContext.Set("subscriptionid", CurrentSubscriptionId);
    else
        NLog.MappedDiagnosticsContext.Set("subscriptionid", null);

    Logger.Info("Begin Request {1} {0}", controllerContext.Request.RequestUri.PathAndQuery, controllerContext.Request.Method);
    return base.ExecuteAsync(controllerContext, cancellationToken);
}
```

<p>We are overriding the ExecuteAsync method, which is the method that is invoked to start processing the request, and set the subscriptionid property for logging purposes. <b>From this point, all logs will include this new property</b>. My NLog target is defined in configuration as this</p>

```xml
<target xsi:type="File" name="f" fileName="${basedir}/App_Data/Logs/App.log"
    layout="${longdate} [T:${threadid}] [L:${uppercase:${level}}] [S:${mdc:item=subscriptionid}] ${message} ${exception:format=tostring}" 
    archiveFileName="${basedir}/App_Data/Logs/Archive/App_${shortdate}.log"
    archiveEvery="Day"
    maxArchiveFiles="30"                
    />

```

<p>The magic happens using <b>the <a href="https://github.com/nlog/NLog/wiki/Mdc-Layout-Renderer">layout render ${mdc}</a> ${mdc:item=subscriptionid}, which expands the property subscriptionid in the log statement</b>. This is great, since to have the subscriptionid emitted in log files, I don't have to specify the subscriptionid property in all my Logger invocations. Layout renders are macros, and you can know more about layout renders <a href="https://github.com/nlog/NLog/wiki/Layout-Renderers">here</a>.</p>
<p>&nbsp;</p>
<p>Now let's imagine this code in a ASP.Net Web API 2 controller</p>

```cs
[HttpPut, Route("{id:int}/start")]
public async Task<IHttpActionResult> Start(int id) 
{
    Integration integration = await Db.MyIntegrations.FirstOrDefaultAsync(x => x.Id == id);
    if (integration == null) 
    {
        Logger.Warn("The user is trying to start an integration with id={0}, which it doesn't belongs to him", id); 
        return NotFound();
    }  
    
    integration.IsActive = true;
    await Db.SaveChangesAsync();

    Logger.Info("Integration {0} is now active and will start to process", id);

    return Ok();
}

```

<p>What I want to notice here is, first that I am using async API, and second that I am using Entity Framework 6, which already supports async API, which means that the code in this action will eventually jump from thread to thread while processing the current request. <b>So, the action will start to process in a given thread, and while it is awaiting for the first database call FirstOrDefaultAsync, the thread will be released to the thread pool instead of being blocked awaiting for the database to return. When the database returns, the action will continue to run in another thread from the thread pool</b>. The same process happens when invoking the method SaveChangesAsync. <b>The major advantage of this async paradigm, is scalability, releasing threads that are not doing any job, allowing to use them to process otherrequests. Do not think that your code will run faster by using the async API</b>.</p>
<p>&nbsp;</p>
<p>So, the web request will be served using different threads, and this is a problem to my main goal of having the subscriptionid in all my logs. Why? Because <b>the MappedDiagnosticsContext class uses the thread local storage (TLS) to keep the dictionary with the properties, and the TLS does not flow across over async points, which means that when the first database call returns, the thread that will run the continuation it will not have anymore the subscriptionid in the TLS</b>. This behavior relates <a href="http://blogs.msdn.com/b/pfxteam/archive/2012/06/15/executioncontext-vs-synchronizationcontext.aspx">how .Net deals with ExecutionContext</a>, which is not an easy subject.</p>
<p>&nbsp;</p>
<p>To achieve our goal <b>we need to store the dictionary in something that will flow across async points. One possible solution is to use the ancient <a href="http://msdn.microsoft.com/en-us/library/system.runtime.remoting.messaging.callcontext(v=vs.110).aspx">CallContext</a></b>. Let's do this in a new class that I will name MappedDiagnosticaLogicalContext</p>

```cs
 namespace NLog 
 {
    public static class MappedDiagnosticsLogicalContext
    {
        private const string LogicalContextDictKey = "NLog.MappedDiagnosticsLogicalContext";

        private static IDictionary<string, string> LogicalContextDict
        {
            get
            {
                var dict = CallContext.LogicalGetData(LogicalContextDictKey) as ConcurrentDictionary<string, string>;
                if (dict == null)
                {
                    dict = new ConcurrentDictionary<string, string>();
                    CallContext.LogicalSetData(LogicalContextDictKey, dict);
                }
                return dict;
            }
        }

        public static void Set(string item, string value)
        {
            LogicalContextDict[item] = value;
        }

        public static string Get(string item)
        {
            string s;

            if (!LogicalContextDict.TryGetValue(item, out s))
            {
                s = string.Empty;
            }

            return s;
        }
    }
 }
```

<p>Now we can rewrite our base controller to use this class instead of the class MappedDiagnosticsContext</p>

```cs
NLog.MappedDiagnosticsLogicalContext.Set("subscriptionid", SubscriptionId);
```

<p>But we also need a custom layout render that can read and expand this items</p>

```cs
 namespace NLog.LayoutRenderers
 {
    [LayoutRenderer("mdlc")]
    public class MdlcLayoutRenderer : LayoutRenderer
    {
        [RequiredParameter]
        [DefaultParameter]
        public string Item { get; set; }

        protected override void Append(StringBuilder builder, LogEventInfo logEvent)
        {
            string msg = MappedDiagnosticsLogicalContext.Get(this.Item);
            builder.Append(msg);
        }
    }
 }
```
<p>This new layout render can be used like this ${mdlc:item=subscriptionid} (mdlc means mapped diagnostics logical context). We need to change our NLog config file, replacing all ocurrences of ${mdc:} by ${mdlc:}.</p>

<p>Finally, and <b>since we are using a custom layout render we have to register it, in the startup of the application</b></p>

```cs
NLog.Config.ConfigurationItemFactory.Default.LayoutRenderers.RegisterDefinition("mdlc", typeof(MdlcLayoutRenderer));
```

<p><b>I made a <a href="https://github.com/NLog/NLog-Contrib/pull/7">pull request to NLog.Contrib project</a> to incorporate this new feature, and it was already merged and incorporated it in the NuGet NLog.Contrib. So, just grab the Nuget and start using without the need to know the internals</b>.</p>