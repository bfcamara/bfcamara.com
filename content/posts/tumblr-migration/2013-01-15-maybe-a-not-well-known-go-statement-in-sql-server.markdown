---
layout: post
template: post
date: 2013-01-15
tags:
- sql server
title: "Maybe a not well known GO statement in Sql Server Management Studio"
slug: /post/40595247187/maybe-a-not-well-known-go-statement-in-sql-server
description: "Maybe a not well known GO statement in Sql Server Management Studio"
---
<p>Recently it was launched a new blog targeted to the community involved with Microsoft Sql Server (<a href="http://www.sql.pt" title="sql.pt" target="_blank">www.sql.pt</a>). The blog was launched by a friend of mine, Pedro Correia,  which is a Sql Server 2008 Microsoft Certified Master (I think it's the only one in Portugal).</p>&#13;
<p>Well, first of all let me say that I consider myself a guy which is well above the average in terms of Sql Server and T-SQL. Since the beginning of my career, which was in 1999, I had to deal with Sql Server and I have been involved in projects that I had to understand the internals and write all the T-SQL by hand.</p>&#13;
<p>I was reading a blog post on this new blog when I saw this snippet of code</p>&#13;

```sql
CREATE TABLE T1 (C1 INT);
GO
INSERT t1 SELECT CAST(RAND() * 100 AS INT);
GO 27
INSERT t1 VALUES (310),(450),(945);
```

<div><span> </span></div>&#13;
<p>At first glance, there is one thing in this script that surprised me a lot:</p>&#13;
<p><strong>   GO 27</strong></p>&#13;
<p>My first reaction was, this script doesn't  run, since the syntax it's not valid. I copied and pasted to my Management Studio, with a connection to a Sql Server 2012, and Bang! It runs! Basically it runs 27 times the insert that precedes this GO statement.</p>&#13;
<p>Oh my god: how I couldn't know this? I checked the<a href="http://msdn.microsoft.com/en-us/library/ms188037.aspx" title="GO statement documentation" target="_blank"> documentation of the GO statement</a>  and here what I read</p>&#13;
<blockquote>&#13;
<p><em> GO [count]<br /><br />  count      Is a positive integer. The batch preceding GO will execute the specified number of times.</em></p>&#13;
</blockquote>&#13;
<p><strong>I felt so frustrated because I didn't know about this optional argument that I started to check the documentation of previous versions</strong>. Basically I was trying to justify my ignorance. The Sql Server 2005 already supported this syntax. Got dammit! However, the documentation available for Sql Server 2000 in <a href="http://msdn.microsoft.com/en-us/library/aa258908(v=sql.80).aspx" title="GO statement documentation in Sql Server 2000" target="_blank">here</a> does not reference any optional count argument. Well, I don't have a chance to test a Sql Server 2000 right now, but I believe that this is something that was introduced with the Sql Server 2005. I started my career with Sql Server 7...bla..bla..bla... No excuses! I am an ignorant! But I didn't remember to be presented to this feature.</p>&#13;
<p><strong>The fact is that this optional count argumenf of GO statement it's useful, particular when testing some situations in Management Studio, when we need to load some data for the tests being run. We can always use a while statement to do some repetitions, although this GO [count] it's much more elegant.</strong></p>&#13;
<p><strong>As a final remark, the GO is not a Transact-SQL statement; it is a command recognized by the sqlcmd and osql utilities and SQL Server Management Studio Code editor.</strong></p> 