---
layout: post
template: post
date: 2014-04-28
tags:
- Azure
- node
- csharp
- software development
title: "Send a message to an Azure Service Bus Queue with C# and reading it in Node - Interoperability problems"
slug: /post/84113031238/send-a-message-to-an-azure-service-bus-queue-with
description: "Send a message to an Azure Service Bus Queue with C# and reading it in Node - Interoperability problems"
---
<p>The other day someone asked me to look at some code in Node. Basically, the code was reading a message from an Azure Service Bus queue. &nbsp;There was nothing wrong with the code, however the person who asked me to help him told me <strong>that the message that he was getting from the queue it wasn't exactly&nbsp;equal to the one that was being sent</strong>. The message was being written in C#, using the Azure Service Bus SDK.&nbsp;Looking at the C# code, the message was being written with the following code</p>
<p><figure class="tmblr-full" data-orig-height="137" data-orig-width="500" data-orig-src="./727679f11d8524c7b5d05f5c5848ab065e01e96c12a7075b68b139e9d01f001e.png"><img alt="image" src="./11afa1b6a2d00c36ccad59cb6bc1905a86fb68cd830117db6d27d60f74d78573.png" data-orig-height="137" data-orig-width="500" data-orig-src="./727679f11d8524c7b5d05f5c5848ab065e01e96c12a7075b68b139e9d01f001e.png"></figure></p>
<p></p>
<p></p>
<p><span>Nothing special. We are just sending in the message's payload a string representation of the JSON object { 'a': '1'}&nbsp;</span></p>
<p><span>And the Node code is reading the message with the following code</span></p>
<p><figure class="tmblr-full" data-orig-height="269" data-orig-width="500" data-orig-src="./b3ea2612b3baed18402b3690017d660e196a0da4ae21d8c1446ddab0eecf9e12.png"><img alt="image" src="./3ef3a9a43f9b3aa834d9dee71a0c8139544a78ba0e5a25667abc05378a29f9f2.png" data-orig-height="269" data-orig-width="500" data-orig-src="./b3ea2612b3baed18402b3690017d660e196a0da4ae21d8c1446ddab0eecf9e12.png"></figure></p>
<p></p>
<p><span>When we run the Node code we're getting the following</span></p>
<p><figure class="tmblr-full" data-orig-height="202" data-orig-width="500" data-orig-src="./270f5bf27de86c4758f1e199c0c1c3935a4e563a3886bb75fed027acb6175065.png"><img alt="image" src="./21426418acf3f2f909ec7659aeaae52bbc91819820fd762781c733769d296a60.png" data-orig-height="202" data-orig-width="500" data-orig-src="./270f5bf27de86c4758f1e199c0c1c3935a4e563a3886bb75fed027acb6175065.png"></figure></p>
<p></p>
<p><strong>We are getting an exception when we try to parse the body of the message as JSON</strong> because the body of the message we are receiving is</p>
<p><code>"@\u0006string\b3http://schemas.microsoft.com/2003/10/Serialization/?\u000b{ \"a\": \"1\"}"</code></p>
<p>And not just what we've sent in C# "{ \"a\": \"1\"}". So, what's happening here?</p>
<p><strong>The problem here is that we are using the BrokeredMessage's constructor that accepts an object as its argument</strong></p>
<p><code>new BrokeredMessage(payload)</code></p>
<p>Let's look at its <a href="http://msdn.microsoft.com/en-us/library/hh322644.aspx" target="_blank">documentation&nbsp;</a></p>
<blockquote>
<p><em>Initializes a new instance of the BrokeredMessage class from a given object by using DataContractSerializer with a binary XmlDictionaryWriter.</em></p>
</blockquote>
<p></p>
<p><strong>So, the payload is being serialized using DataContractSerializer with a binary XmlDictionaryWriter and this is the cause that explains why the body of the message&nbsp;has in its start the type indication</strong></p>
<p><code>@\u0006string\b3http://schemas.microsoft.com/2003/10/Serialization/?\u000b</code></p>
<p>How to fix it?</p>
<p><span>We have two options to fix the problem:&nbsp;</span></p>
<ol><li><span>when reading the message in Node strip the part that we are not interested in</span></li>
<li><span></span><strong>or guarantee that the body of the message is just what we are sending, nothing more</strong></li>
</ol><p>I will focus on the 2nd fix because in my opinion is cleaner. <strong>To guarantee tha the body of the message is just what we are sending </strong><strong>we have to use other BrokeredMessage's constructor, passing a Stream as an argument</strong></p>
<p><code>public BrokeredMessage(Stream messageBodyStream, bool ownsStream)</code></p>
<p>Here what it's said in <a href="http://msdn.microsoft.com/en-us/library/hh330764.aspx" target="_blank">documentation</a></p>
<blockquote>
<p><em>Initializes a new instance of the BrokeredMessage class using the supplied stream as its body</em>.</p>
</blockquote>
<p></p>
<p>So, in this case we have full control what will be the body of the message. Let's rewrite our C# code to use this constructor</p>
<p><figure class="tmblr-full" data-orig-height="137" data-orig-width="500" data-orig-src="./91bd4503ed1977e3e917c28c5f797c1f0c7f76e3ddc07ddd8c55f3068dfadc9c.png"><img alt="image" src="./54c0abde89faa6700947a34f4dad8d6ce9294f58e6613bde128e7e164486cacf.png" data-orig-height="137" data-orig-width="500" data-orig-src="./91bd4503ed1977e3e917c28c5f797c1f0c7f76e3ddc07ddd8c55f3068dfadc9c.png"></figure></p>
<p></p>
<p>The main difference here is that<strong> we are converting our payload string to a stream and use this stream to build our brokered message</strong>.</p>
<p><span>After sending the message to the queue, let's run again the node code</span></p>
<p><figure class="tmblr-full" data-orig-height="56" data-orig-width="500" data-orig-src="./52a4cb4908fe0d98be2000f01de2e719622eb52e1750cd7830cea3c236562323.png"><img alt="image" src="./92a6e96cd1eb4af0aabce96aefb4e8b79e793830d870a7610957064ffc54503e.png" data-orig-height="56" data-orig-width="500" data-orig-src="./52a4cb4908fe0d98be2000f01de2e719622eb52e1750cd7830cea3c236562323.png"></figure></p>
<p></p>
<p></p>
<p><strong>Now we are able to parse the body as JSON and everything is working as expected.</strong></p>
<p></p>
<p><strong>Conclusion</strong>: we have to be careful when we have two different worlds in the ends (.Net C# &nbsp;vs Node) and double check for interoperability problems.</p>