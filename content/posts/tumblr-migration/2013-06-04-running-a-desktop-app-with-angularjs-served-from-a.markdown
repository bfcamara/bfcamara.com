---
layout: post
template: post
date: 2013-06-04
tags:
- owin
- katana
title: "Running a desktop app with AngularJS served from a Windows Service with Katana (OWIN)"
slug: /post/52138881551/running-a-desktop-app-with-angularjs-served-from-a
description: "Running a desktop app with AngularJS served from a Windows Service with Katana (OWIN)"
---
<p>Nowadays, <strong>when developing desktop applications for Windows, we have many options when choosing the .Net technology</strong> to use:</p>
<ul>
<li>Windows Prsentation Foundation (WPF) - in my opinion, this is the most consensus choide</li>
<li>Windows Forms</li>
<li>Silverlight</li>
</ul>
<p>If the target is Windows 8 we can add even more choices to the pile, for example using HTML5 and Javascript, due the support of the new WinRT architecture.</p>
<p>Recently I had to analyze a scenario where a windows service should be developed, which would be responsible to run some kind of scheduled tasks -&nbsp;polling a SaaS service on the public internet, and integrate some data with on-premises systems. Additionally, a graphical tool should also be developed, to allow&nbsp;to&nbsp;configure some aspects of the windows service (instead of editing configuration files by hand).</p>
<p>For the UI tool, my first approach was to choose a WPF application. But I have no experience at all with WPF, and as a developer I am more comfortable with the web stack development, which means that on the client side I would like to use Html5 and Javascript, and the UI tool would be running on the browser.</p>
<p>Since <a href="http://bfcamara.com/post/50641543547/im-in-love-with-angularjs-here-is-the" target="_blank">I'm in love with AngularJS</a>, my wish was to develop the tool using <a href="http://angularjs.org/" title="AngularJS" target="_blank">AngularJS</a>. But I don't want to complicate the installation process, requiring my tool to be hosted in Internet Information Service (IIS). So, what I really want is to <strong>install my window service, which besides the integration logic, should also host my AngularJS application</strong>.</p>
<p>Well, in fact, what <strong>I really need is to have some kind of embedded web server</strong> that supports:</p>
<ul>
<li>Static files - (html, images, css and javascript files)</li>
<li>AspNet Web Api - I want to use WebApi to return the data to the graphical tool</li>
</ul>
<p>With this goal in mind, it's time to meet <a href="owin.org" title="OWIN" target="_blank">OWIN</a> (Open web Interface for .Net) specification and the <a href="http://katanaproject.codeplex.com/" title="Katana Project" target="_blank">Katana Project</a>.</p>
<p>From the OWIN site</p>
<blockquote>
<div><em>OWIN defines a standard interface between .NET web servers and web applications. The goal of the OWIN interface is to decouple server and application, encourage the development of simple modules for .NET web development, and, by being an open standard, stimulate the open source ecosystem of .NET web development tools.</em></div>
</blockquote>
<p>So, basically, <strong>OWIN is a spec which allows components to be reused on different web servers</strong>.</p>
<p><strong>The <a href="http://katanaproject.codeplex.com/" title="Katana Project" target="_blank">Katana Project</a> is a collection of projects to support OWIN with various Microsoft components</strong>. It is also a command line application for running self-host servers. To have a great overview of the Katana project read <a href="http://www.asp.net/vnext/overview/owin-and-katana/an-overview-of-project-katana" title="Overview of Katana Project" target="_blank">this post</a>.</p>
<p>Ok, stop theory, and show me the&nbsp;<strong>how can I use Katana to embed a small web server on my windows service</strong>. which allows me to build an AngularJS app. Here we go!</p>
<p>First, let's create a new Visual Studio project, choosing the type Windows Service</p>
<p><img alt="image" src="./ba58a9f8272b045ba727965042111286ce299a37ef9a60b722778d2b235dfeeb.png" /></p>
<p>&nbsp;</p>
<p>Next, let's make some modifications to the project to allow us to test without having to install the windows service:</p>
<ul>
<li>Change the project type to Console Application (Project Properties)</li>
</ul>
<p><img alt="image" src="./685628c1f02450defe6cd397a96a5e92ed37cc09216870e99cf05fee69737b30.png" /></p>
<p></p>
<ul>
<li>Change the Start and Stop methods of the windows service to allow calls from a console app</li>
</ul>
<p><img alt="image" src="./41acc09a4c9d3528566f32f4923165f7e056aa531f7c6555273707cd50e4481b.png" /></p>
<p></p>
<ul>
<li>Add a conditional compilation statement #if to allow to run in Debug as a console application</li>
</ul>
<p><img alt="image" src="./4b1983751332a2a74df37928dd4800fdf148ee5e7fcdb4331f9ca18fa0efe125.png" /></p>
<p></p>
<p>Now let's start to embed our web server. We will need some nuget packages</p>
<p><strong>The first NuGet we will need it's the Microsoft.Owin.Hosting</strong>, which allow us to have a basci OWIN hosting in our application</p>
<p><img alt="image" src="./f72da0f6f629b439539a1f54f2785eeb95c16c6d54af194d050a2e83e0dd67b0.png" /></p>
<p></p>
<p>Notice that the package is a pre-release version, so we should change the option in the drop down "Stable Only" to "Include Prerelease". To create our web application host, we just need to modify out internal start&nbsp;and call the method WebApplication.Start, indating the listen url as parameter (<a href="http://localhost:12345">http://localhost:12345</a>).</p>
<p><img alt="image" src="./8546dac228afbdd2dd033cb2bc32c13e4dee6ed39b6e5e027a24bbb9111e9a52.png" /></p>
<p></p>
<p>The Start method is a generic method, where we have to tell what is the class responsible to do the initial startup, which in our case it is the WebStartup class. By convention, the method to be called it is named Configuration, receiving the application builder object as an argument</p>
<p><img alt="image" src="./84c122c7ea93c9759946894afa9f50a952aade724c965378c88c734bda674f77.png" /></p>
<p></p>
<p>Let's try to run our application, hitting F5, to see what happens</p>
<p><img alt="image" src="./f8d0e00cc52e5a7603602ad00480b6561ba3beb43aa08ae27a9ec4c37f466147.png" /></p>
<p></p>
<p>We are getting an exception, telling us that the HTTP Listener is not found.</p>
<blockquote>
<p><em>Could not load file or assembly 'Microsoft.Owin.Host.HttpListener' or one of its dependencies. The system cannot find the file specified.</em></p>
</blockquote>
<p>&nbsp;</p>
<p>Until now, what we did was just adding the hosting capabilies to our application. We need to specify the OWIN component that should listen for&nbsp;and serve HTTP requests. <strong>Let's add the Microsoft.Owin.Host.HttpListener NuGet, which is the OWIN component that uses the Microsoft Windows HTTP protocol stack</strong> <strong>(http.sys)</strong></p>
<p><img alt="image" src="./73097b777f7eb35bda84337b9fd4a33a0bceaa16963745504997af246a3c32e5.png" /></p>
<p></p>
<p>Let's hit F5 again and see what happens</p>
<p><img alt="image" src="./03dcd8da42acf4af32e4f2c5577b047ee6ba50908577382385c16ffdfd354284.png" /></p>
<p></p>
<p>As you can see nothing happens, and the exception disapears. <strong>However I want to have a quick way to check if the hosting is correct. To do this, we will add another NuGet Microsoft.Owin.Diagnostics</strong>, which will allows to have a quick diagnostics</p>
<p><img alt="image" src="./5051103f09e5fe807d7b638a455a24b9343d4e335178d060bbb5535eb85bb81c.png" /></p>
<p></p>
<p>Next, we need to tell to our web application to use the diagnostics capabilities, calling the method UseTestPage() during the configuration phase</p>
<p><img alt="image" src="./8502e137aa9792fa16dd7918e7ae7978e1680622b528b5c9041fc43fa1155582.png" /></p>
<p></p>
<p>Let's hit F5 again, and open a browser and type our url on the address bar</p>
<p>&nbsp;</p>
<p><img alt="image" src="./1cec5ac84c4e06ae3f04eeb94e691014312b260c0c1491c335a481b966227a65.png" /></p>
<p></p>
<p>Cool! Now we have an http listener which serves always this simple test page, greeting "Welcome to Katana". <strong>Now we need to add another NugGt, Microsoft.Owin.StaticFiles, which will allow us to serve our static files (Html, CSS, Javascript files, etc.)</strong>. For some reason, this nuget is not being returned in the search result when using the window "Manage NuGet Package". So, we will have to use the Package Manager Console and use the command</p>
<p><code>Install-Package Microsoft.Owin.StaticFiles -Version 0.20-alpha-20220-88 -Pre</code></p>
<p><img alt="image" src="./2ffdeedfe48935d153af82fae3c1e9dc207dbb8f60bd9aba151a43f0c59e27ad.png" /></p>
<p>&nbsp;</p>
<p>Now we need to configure our static files component to tell the path where the static files are. In my case I choose to create a folder webdir to be the static files container, and it will be located in the same directory&nbsp;of the executing assembly</p>
<p><img alt="image" src="./c6bc741cab3099cdf2e114b592d7af531ff75bfd77a2657ed4adfce103b310de.png" /></p>
<p></p>
<p>Let's add an Index.html file to webdir folder and try to navigate to this page in the browser (do not forget to mark this file to be added to the output directory)</p>
<p><img alt="image" src="./88bda9c57f12ff8ed27e0f29d28c726f39ced7e43526eaf20a1733c70ed6917b.png" /></p>
<p></p>
<p>&nbsp;</p>
<p><img alt="image" src="./8b22239eb7fd3a68934fe063eaa8c3585f8395e5841e4be2e2642ff2c3f32bc9.png" /></p>
<p></p>
<p>Hit F5 and browse to http://localhost:12345</p>
<p><img alt="image" src="./4e79d4c0b1594e59b0a1087ad1180f9171a3fee735303ec17728bd61b1264670.png" /></p>
<p></p>
<p>Very good. Now we can serve static files. Half of our problem is solved, since an AngularJS application it's just a bunch of Html, javascript, CSS, and imagesfiles.</p>
<p>But if you remember, we need also to use WebApi, which is the way to return some data to the app. <strong>Let's add another NuGet, Microsoft.AspNet.WebApi.Owin, which will allow us to host WebApi controllers and provide a REST api on our application</strong>.</p>
<p><img alt="image" src="./338e0059bf7e33ae814470dae83157e500b72d0f528ca70206d06b8988b0fa78.png" /></p>
<p></p>
<p>Now we need to configure the WebApi, telling the route pattern to use.</p>
<p><img alt="image" src="./8b57815a11fc0b00ed8486c176719a8cfed5ee3b378231fd5527fe73afce9007.png" /></p>
<p></p>
<p>Basically we configured to use the route /api/{controller} tou our WebApi controllers. Let's add a simple HomeController with the Get method, returning a simple string.</p>
<p><img alt="image" src="./ce6139d68f067fe8fc67a1698d2ac30988ed0015df985d016a65c58894df6629.png" /></p>
<p></p>
<p>Let's Hit F5 and browse to the url http://localhost:12345/api/home</p>
<p><img alt="image" src="./006617ba75714298b6bc0ab3d4c1fc8f16806fff744637e20aa6d963c8f20907.png" /></p>
<p>The WebApi&nbsp;content-negotiation resulted in a json fle. Let's click Open to see the content</p>
<p><img alt="image" src="./3f0b9d1cc0a56bc641b7bf0f730800b7bad06aa74422b29f8981adfa622ecdac.png" /></p>
<p></p>
<p>Bingo. Our application now&nbsp;handles WebApi.</p>
<p>Finally, to install this as a windows service, we just need to add a project installer</p>
<p><img alt="image" src="./29e84d3b24b13af8dc5c5f9da79dfe5d1db29fba6f81bd7d351d42ab1ae8cba3.png" /></p>
<p></p>
<p>Compile in release mode, install the windows service (using installutil.exe) and you are ready to add your AngularJS application, which will be served by the windows service process, without the need of Internet Information Service (IIS). With this, now I can&nbsp;consider to build my configuration tool (to run on desktop) using AngularJS and WebApi.</p>
<p>You can download the full code in <a href="https://github.com/bfcamara/KatanaWinSvcSample" target="_blank">my github account</a>.</p>