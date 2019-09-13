---
layout: post
template: post
date: 2014-01-13
tags:
- angularjs
- Visual Studio
- test
title: "Running Angular Tests in VS Test Explorer with Chutzpah"
slug: /post/73198752368/running-angular-tests-in-vs-test-explorer-with
description: "Running Angular Tests in VS Test Explorer with Chutzpah"
---
<p><span>The ecosystem around Angular is typically following the <a href="http://yeoman.io/">Yeoman </a>workflow. This workflow is based on three tools:</span></p>
<ul><li>Yo - scaffolding tool</li>
<li>Grunt - taskrunner, used to build and test a web app</li>
<li>Bower - dependency management system</li>
</ul><p>I have nothing against this workflow, but since I am a .Net developer, I am used to another tools which reside inside Visual Studio. And I believe that the majority of .Net developers has some reluctance to install node on their machines, which is a requirement installation for all of that tools. We, .Net developers, are accustomed to:</p>
<ul><li>use Project templates to scaffold and bootstrap the app, or Project Item templates with T4 templates to scaffold some specific items</li>
<li>MSBuild as the taskrunner. For running tests we are used to do it inside Visual Studio using the Test Explorer</li>
<li>Nuget as our dependency management system</li>
</ul><p>I&rsquo;m passionate about Angular. But &nbsp;I&rsquo;m passionate about Visual Studio either. &nbsp;So, regarding to angular tests, I would like to run them using Test Explorer, instead of grunt. Suppose that I have this angular app inside Visual Studio. &nbsp;It is a very simple app, created from template Empty ASP.NET application, with a Add Unit Tests checkbox checked.</p>
<p><figure class="tmblr-full" data-orig-height="265" data-orig-width="500" data-orig-src="./ae02d456ca727816e279bb7b3ef46b7b027f226ab187583182c3a0a7d4a1a1c3.png"><img alt="image" src="./d5f810f42a14c20ab8eaaae157fdb0c74aa09ff16ab859a0a85ad57f2da3e97b.png" data-orig-height="265" data-orig-width="500" data-orig-src="./ae02d456ca727816e279bb7b3ef46b7b027f226ab187583182c3a0a7d4a1a1c3.png"></figure></p>
<p><figure class="tmblr-full" data-orig-height="515" data-orig-width="500" data-orig-src="./faeff56a47d15ba377c182a47c0339ab4f656766bcf79b7ced2df61e3c563c0f.png"><img alt="image" src="./74e1cd8da0497ca4ca887d1e447b49653118a3b151493f4b91ab568d78fe78dc.png" data-orig-height="515" data-orig-width="500" data-orig-src="./faeff56a47d15ba377c182a47c0339ab4f656766bcf79b7ced2df61e3c563c0f.png"></figure></p>
<p>After that, I've added the nuget packages Angular Core and Twitter Bootstrap 3.</p>
<p><figure class="tmblr-full" data-orig-height="123" data-orig-width="500" data-orig-src="./9cda167584cbf2a8825cb5895298eec3b0164d03317f3ad6d5bcce193126e42e.png"><img alt="image" src="./bf8fc33dec62bc9bc7786444a477dcfc59aab97d76f67977875086fb01a5e97e.png" data-orig-height="123" data-orig-width="500" data-orig-src="./9cda167584cbf2a8825cb5895298eec3b0164d03317f3ad6d5bcce193126e42e.png"></figure></p>
<p><figure class="tmblr-full" data-orig-height="169" data-orig-width="500" data-orig-src="./698d3358e9f0c6daf69fea0965e61af16a9f5ca9d4b882cee905ea519fb98c38.png"><img alt="image" src="./100089fda4f5800a63758749dc0f61a0e8e198e1bbe67d8d9cb572fd01d82a80.png" data-orig-height="169" data-orig-width="500" data-orig-src="./698d3358e9f0c6daf69fea0965e61af16a9f5ca9d4b882cee905ea519fb98c38.png"></figure></p>
<p>Then, I've created an Index.html &nbsp;and an app.js file</p>
<p><figure class="tmblr-full" data-orig-height="516" data-orig-width="400" data-orig-src="./5806c68dd2fd13f76ce224743a854910ae362b8e9dbd4a528e616e692465c6d6.png"><img alt="image" src="./3579eed8fcd0c0920af05c6c0811540bc2391a31b577afb11a5f173160a689ef.png" data-orig-height="516" data-orig-width="400" data-orig-src="./5806c68dd2fd13f76ce224743a854910ae362b8e9dbd4a528e616e692465c6d6.png"></figure></p>
<p></p>
<p></p>
<p>Here is the HTML to show a table with a list of tasks</p>
<p><figure class="tmblr-full" data-orig-height="540" data-orig-width="500" data-orig-src="./46edad98ddd2ae4ce4c0b8bc31b780ab96644781496f0436e130481c98efe633.png"><img alt="image" src="./2c5df73bc7f9e04fb4957ceaac30ed70ea8970b438d1156ac53f8de15fcb041a.png" data-orig-height="540" data-orig-width="500" data-orig-src="./46edad98ddd2ae4ce4c0b8bc31b780ab96644781496f0436e130481c98efe633.png"></figure></p>
<p></p>
<p><span>And here is the js code, which includes a controller MainCtrl responsible to return the tasks to the view, using a service dataService which will encapsulate the call to the backend (for now it returns an array hardcoded).</span></p>
<p><figure class="tmblr-full" data-orig-height="358" data-orig-width="500" data-orig-src="./58f68778f56b1784ada6e8a06b6cdc63ceeb92ae32ba16490515833242c43cda.png"><img alt="image" src="./3be12f5c5d007195718351ac46d0d3449bbde8925edfdc1d78bba9764f737682.png" data-orig-height="358" data-orig-width="500" data-orig-src="./58f68778f56b1784ada6e8a06b6cdc63ceeb92ae32ba16490515833242c43cda.png"></figure></p>
<p></p>
<p>Hit F5 and here is the result</p>
<p><figure class="tmblr-full" data-orig-height="329" data-orig-width="500" data-orig-src="./250d611b1715d4b14010ac61cb15d47131e8c12cfbd0b1677a0b81bcfbabba6e.png"><img alt="image" src="./24148788906d3b5e2b2941aafcfcccafb698c66ba9c71106795aff61d0cf4055.png" data-orig-height="329" data-orig-width="500" data-orig-src="./250d611b1715d4b14010ac61cb15d47131e8c12cfbd0b1677a0b81bcfbabba6e.png"></figure></p>
<p></p>
<p><span>Next, in my test project , I&rsquo;ve added the packages Angular Core and <a href="http://pivotal.github.io/jasmine/">Jasmine</a> (the test framework).</span></p>
<p><span>&nbsp;<figure class="tmblr-full" data-orig-height="172" data-orig-width="500" data-orig-src="./6ae86e9985bf29136105589a9b906246308943f9862a05aebfa064631b88c286.png"><img alt="image" src="./b4be010687c328bcaa756094e3c5b938d819291f24205af75a40f984970fbc3e.png" data-orig-height="172" data-orig-width="500" data-orig-src="./6ae86e9985bf29136105589a9b906246308943f9862a05aebfa064631b88c286.png"></figure></span></p>
<p></p>
<p><span>Then I&rsquo;ve added my test, which is a normal angular controller test, written in jasmine, asserting that during the initialization of the MainCtrl the scope.tasks is set.</span></p>
<p><span><figure class="tmblr-full" data-orig-height="537" data-orig-width="432" data-orig-src="./b21824cdc4e527787bf25d25ff683e4326402d282dc464df3c8a10a1d6fc8d7d.png"><img alt="image" src="./cd0713106ccfe8acdb19747a6d76578a39136b9238d354e982bf950aa3e56c08.png" data-orig-height="537" data-orig-width="432" data-orig-src="./b21824cdc4e527787bf25d25ff683e4326402d282dc464df3c8a10a1d6fc8d7d.png"></figure></span></p>
<p></p>
<p><span>&nbsp;</span></p>
<p><figure class="tmblr-full" data-orig-height="364" data-orig-width="500" data-orig-src="./91d7fe404373e1bc869d82bf23d1221b060dd6a95e6ecace3517b258f45e9470.png"><img alt="image" src="./5558596197a3110da806341a6c75cdca980a7f2b8a1fb3cca6b93ea345a8b3e7.png" data-orig-height="364" data-orig-width="500" data-orig-src="./91d7fe404373e1bc869d82bf23d1221b060dd6a95e6ecace3517b258f45e9470.png"></figure></p>
<p><span>To run the tests in the browser, I&rsquo;ve added an html file and use the jasmine runner itself to run the tests.</span></p>
<p><span><figure class="tmblr-full" data-orig-height="500" data-orig-width="356" data-orig-src="./88d53e4601536575135ca83ad6da820099c538bf2899d231adafc32efbc2a67e.png"><img alt="image" src="./b44d7a78bdefb104c4862d5a496611fad4668cfc4ca45a7884b5927869a82fb5.png" data-orig-height="500" data-orig-width="356" data-orig-src="./88d53e4601536575135ca83ad6da820099c538bf2899d231adafc32efbc2a67e.png"></figure></span></p>
<p></p>
<p><span>&nbsp;</span></p>
<p></p>
<p><figure class="tmblr-full" data-orig-height="404" data-orig-width="500" data-orig-src="./dcc2c15d9afde4ff2ade70ad66c07d86ffc639511332b816ebb01d3a48d95489.png"><img alt="image" src="./9c48698fced615814a9a23b2e11a1e3961f42c014778d716d628987f14adad69.png" data-orig-height="404" data-orig-width="500" data-orig-src="./dcc2c15d9afde4ff2ade70ad66c07d86ffc639511332b816ebb01d3a48d95489.png"></figure></p>
<p></p>
<p>And here is the result when opening this html file in the browser</p>
<p><figure class="tmblr-full" data-orig-height="334" data-orig-width="500" data-orig-src="./fd70cc056bd1962dd883b35030dc1a2ea97150db5b74b72860290d286135e425.png"><img alt="image" src="./5a7dedc60ab929801280bcfc076e5629e24a6dd22622a5d7e962c410f548a4fa.png" data-orig-height="334" data-orig-width="500" data-orig-src="./fd70cc056bd1962dd883b35030dc1a2ea97150db5b74b72860290d286135e425.png"></figure></p>
<p><span>But my main goal is to run tests using the test explorer, instead of running in the browser.</span></p>
<p><strong><span id="docs-internal-guid-62fa1205-8b67-ebaa-c809-32ae64c5bc6c">Introducing Chutzpa Adapter for Test Explorer</span></strong></p>
<p><span id="docs-internal-guid-62fa1205-8b67-8990-ca78-9d130ccf373e"><a href="http://chutzpah.codeplex.com/" target="_blank">Chutzpa</a> ia a javascript test runner which supports QUnit, Jasmine, and Mocha frameworks, and provides a VS Test Adapter. Let&rsquo;s install it in VS, choosing menu option Tools -&gt; Extensions and Updates</span></p>
<p><span><figure class="tmblr-full" data-orig-height="398" data-orig-width="500" data-orig-src="./b0a0d03abd3fca6b4bcd09ca2c2fbe7d2c2ecc47f2bf6790fdee9b3cecf39ab8.png"><img alt="image" src="./adc28b3bc1f335d1761cd4f4dd02261644d49ac1ece82e841f08f40062e32a5e.png" data-orig-height="398" data-orig-width="500" data-orig-src="./b0a0d03abd3fca6b4bcd09ca2c2fbe7d2c2ecc47f2bf6790fdee9b3cecf39ab8.png"></figure></span></p>
<p></p>
<p></p>
<p><span>Then, search for Chutzpah, and install extension Chutzpah Test Adapter for the Test Explorer.</span></p>
<p><span><figure class="tmblr-full" data-orig-height="233" data-orig-width="500" data-orig-src="./1c12050a1b5251d0ad635de4060df13b5a745e3a966583e50992dff99b1db887.png"><img alt="image" src="./8d173c7fec1bd706fd06f41012219e0be9dde5bf2a7b89e3662d72b738ee93c5.png" data-orig-height="233" data-orig-width="500" data-orig-src="./1c12050a1b5251d0ad635de4060df13b5a745e3a966583e50992dff99b1db887.png"></figure></span></p>
<p></p>
<p>After that, restart Visual Studio , and check the Test Explorer Window.</p>
<p><figure class="tmblr-full" data-orig-height="229" data-orig-width="500" data-orig-src="./800a9ee8d3f4b636c3709517caa3745633e94132d169957bc93e939fe981be30.png"><img alt="image" src="./9b5def50faf7c9cbca16020b9a8571bfb2aad3d1054e2e675cc8807fa8e0b0ed.png" data-orig-height="229" data-orig-width="500" data-orig-src="./800a9ee8d3f4b636c3709517caa3745633e94132d169957bc93e939fe981be30.png"></figure></p>
<p></p>
<p>Ok, so now we have our tests in Test Explorer. Let's try to run them.</p>
<p><figure class="tmblr-full" data-orig-height="510" data-orig-width="500" data-orig-src="./cf705ef2662ecb8aaa76c7e08f725c95b97e8dfc2db34171cb401001af710688.png"><img alt="image" src="./bbab05ddb61e8767239fd9f22d7f428c79f7e2108a6299ff1c84b899d5484214.png" data-orig-height="510" data-orig-width="500" data-orig-src="./cf705ef2662ecb8aaa76c7e08f725c95b97e8dfc2db34171cb401001af710688.png"></figure></p>
<p></p>
<p><span>As you can see, the tests are failing, because Chutzpah is not using the html file to run the tests, hence it is missing some refrences. To fix this, we have to add some comment document references to our test js file</span></p>
<p><figure class="tmblr-full" data-orig-height="382" data-orig-width="500" data-orig-src="./ec38f1c0ed46d5921356069ca3589b2401f023f7e36a699c46c219920e47bc2b.png"><img alt="image" src="./8a25bfc4759f3b03b34e23051b73967d166e0490b8292b7e70e3ffb2fe9e0c9d.png" data-orig-height="382" data-orig-width="500" data-orig-src="./ec38f1c0ed46d5921356069ca3589b2401f023f7e36a699c46c219920e47bc2b.png"></figure></p>
<p></p>
<p>Let's run again our tests.</p>
<p><figure class="tmblr-full" data-orig-height="316" data-orig-width="500" data-orig-src="./1fba4752b88bee680713be7bccc2b24fb8515728dce2b126be3586b953232b1f.png"><img alt="image" src="./fec5b6e42f925850c6466bde26159670f643288812ae7e5a96cf756945e0cfc3.png" data-orig-height="316" data-orig-width="500" data-orig-src="./1fba4752b88bee680713be7bccc2b24fb8515728dce2b126be3586b953232b1f.png"></figure></p>
<p></p>
<p>Voil&aacute;. Now I can run my angular tests in Test Explorer.</p>
<p></p>
<p></p>