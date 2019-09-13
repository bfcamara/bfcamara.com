---
layout: post
template: post
date: 2013-11-26
tags:
- ef
- .net
- database
title: "Entity Framework: Database-First vs Code-First "
slug: /post/68169174771/entity-framework-database-first-vs-code-first
description: "Entity Framework: Database-First vs Code-First"
---
<p><figure data-orig-height="76" data-orig-width="200"><img src="https://66.media.tumblr.com/be344b102fc4d4c9420fb5431737169b/6c7d831abddfbb3a-61/s540x810/cf250efb1159c65b8443cf0448f050b8ab89646a.png" data-orig-height="76" data-orig-width="200"></figure></p>
<p></p>
<p>When working with <strong><a href="http://msdn.microsoft.com/en-us/data/ef.aspx" target="_blank">Entity Framework (EF) </a> <a href="http://msdn.microsoft.com/en-us/data/ee712907#codefirst" target="_blank">Code-First</a>, we define our entity model with classes and mappings in code, and the database is generated from the model</strong>. When we change the model, we can evolve the database using <a href="http://msdn.microsoft.com/en-us/data/jj591621" target="_blank">Migrations</a>.</p>
<p>&nbsp;</p>
<p>In the past, I've used more the <a href="http://msdn.microsoft.com/en-us/data/jj206878" target="_blank">Database- First approach</a> (rather than the Code First). <strong>In the Database-First approach we start to model the database artifacts (tables, views, primary keys, foreign keys, etc.), and then we have the entity model generated from the database</strong>.</p>
<p>&nbsp;</p>
<p>In the earlier versions, EF didn't support the <strong>Code-First approach</strong>. Today, EF already supports this approach, and it has been improved in every new version. I decided to try this approach in my current project, and I would like to enumerate <strong>some advantages</strong>:</p>
<ul><li><strong>all my application "runnable" artifacts are written in C#</strong></li>
<li><strong>no need to have a different database project</strong> to accommodate the data artifacts</li>
<li><strong>faster development workflow</strong></li>
<li><strong>makes me think in advance about "the scripts" that will run to upgrade/downgrade</strong> my database to accomodate model changes. (every migration has an up and down path, where you can run sql migration statements)</li>
</ul><p>&nbsp;</p>
<p>However, I also can enumerate <strong>some disadvantages</strong></p>
<ul><li>if your model is changing a lot, and you are working in a team with more than 3 elements, I believe that the team&nbsp;will <strong>struggle easily with migrations (the migrations folder starts to grow and it get's out of control)</strong></li>
<li><strong>Some annotation attributes are missing</strong>, such as the Index attribute for example. Currently, with EF 6, I cannot decorate my entity properties with an Index annotation. To create an index over a given property, I have to use the fluent API. I tend to prefer the attribute annotations approach.</li>
<li><strong>some lack of control how the database artifacts will be generated</strong></li>
</ul><p>&nbsp;</p>
<p>I believe that those more conventional/traditional developers will continue to use the well known Database-First approach. In my case, since I know both approaches, I will support my <strong>choice based on context/environment: team and project dimension, type of customer, etc. But now I can say that I'm really confident and confortable with the Code-First approach, and certainly I will use it regularly</strong>.</p>