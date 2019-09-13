---
layout: post
template: post
date: 2014-05-29
tags:
- software development
- WebAPI
- API
title: "How much RESTful is your API"
slug: /post/87193730398/how-much-restful-is-your-api
description: "How much RESTful is your API"
---
<p>We all have a tendency to say and assume that our API is RESTful as soon as we expose our services using HTTP with JSON/XML and applying some conventions around the HTTP verbs (GET, POST, PUT, DELETE, etc.). However, this assumption is not correct. I don't want to dig in the <a href="http://www.ics.uci.edu/~fielding/pubs/dissertation/rest_arch_style.htm" target="_blank">REST architectural style</a> to show why this assumption is &nbsp;incorrect; instead I want to share something that I learned recently while I'm preparing for my <a href="http://www.microsoft.com/learning/en-us/mcsd-web-apps-certification.aspx" target="_blank">MCSD certification in Web Apps</a>, in particular while studying for <a href="http://www.microsoft.com/learning/en-us/exam-70-487.aspx" target="_blank">exam 70-487</a>: <a href="http://www.crummy.com/writing/speaking/2008-QCon/act3.html" target="_blank"><strong>The Richardson Maturity Model</strong></a>.</p>
<p><strong>The Richardson Maturity Model describes four levels of maturity for services</strong></p>
<ul>
<li><span><strong>Level 0</strong>. Use <strong>HTTP as a transport protocol</strong> by ignoring the capabilities of HTTP as an&nbsp;</span>application layer protocol. Level 0 services use a single address, also known as endpoint and asingle HTTP method, which is usually POST. SOAP services and other RPC-based protocols are&nbsp;examples of level zero services.</li>
<li>&nbsp;<strong>Level 1</strong>. Identify <strong>resources by using URIs</strong>. Each resource in the system has its own URI by&nbsp;<span>which the resource can be accessed.</span></li>
<li><span></span><strong>Level 2</strong>. Uses the different<strong> HTTP verbs</strong> to allow the user to manipulate the resources and&nbsp;create a full API based on resources.</li>
<li><span><strong>Level 3</strong>. Although the first two services only emphasize the suitable use of HTTP&nbsp;</span>semantics, level 3 introduce <strong>Hypermedia</strong>, an extension of the term Hypertext as a means&nbsp;for a resource to describe their own state in addition to their relation to other resources.</li>
</ul>
<p><span>Martin Fowler has a </span><span></span><a href="http://martinfowler.com/articles/richardsonMaturityModel.html" target="_blank">great post </a><span>in his blog describing in detail these four levels.</span></p>
<p><span>I would risk to say that most of us, when using the Web API project template provided in Visual Studio and following the typical recipes, or using the&nbsp;</span>scaffolding generated code, <strong>most of the time we are building services at level 2</strong>. If we want our services at level 3, we need to introduce Hypermedia elements in our payloads. Maybe we should look to<strong><a href="http://stateless.co/hal_specification.html" target="_blank"> HAL - Hypertext Application Language</a>, which is a simple format that gives a consistent and easy way to hyperlink between resources in our API</strong>.</p>
<p></p>
<p>Roy Fielding, the author of the doctoral dissertation that describes REST, considers that <a href="http://roy.gbiv.com/untangled/2008/rest-apis-must-be-hypertext-driven" target="_blank"><strong>level 3 is required in a REST architectural style</strong></a>. However, I think I'll continue to say that "our API is a RESTful API", even though my services are at level 2.</p>