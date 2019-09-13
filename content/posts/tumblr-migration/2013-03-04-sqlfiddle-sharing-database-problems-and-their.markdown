---
layout: post
template: post
date: 2013-03-04
tags:
- sql
title: "SQLFiddle - sharing database problems and their solutions"
slug: /post/44500610899/sqlfiddle-sharing-database-problems-and-their
description: "SQLFiddle - sharing database problems and their solutions"
---
<p>If you are a web developer, probably you know <a href="jsFiddle.net" title="jsFiddle" target="_blank">jsFiddle</a>. If you don't know you can always start play with it today. <strong>jsFiddle it's an online playground tool where you can test your javascript/css/html problems and share it with others</strong>. Many StackOverflow answers, the answers site for developers, include a link to jsFiddle to demonstrate a problem and/or a solution. jsFiddle became very popular among web developers. There are other online playground tools, but they are tipically focused in just &nbsp;one technology, HTML or CSS or Javascript. jsFiddle combines the three.&nbsp;</p>
<p>&nbsp;</p>
<p><strong>Until recently, I didn't know any tool similar with jsFiddle, but focused on database problems. But there is! It's the <a href="http://sqlfiddle.com" title="SQL Fiddle" target="_blank">SQLFiddle</a>. &nbsp;And if you use jsFiddle you start to recognize the similarities, and in seconds you start to use SQLFiddle without problems</strong>.</p>
<p>The other day I was talking with a guy, who started testing my SQL knowledge and asked me to solve a problem.</p>
<p>Here is the problem:<br /> <br />If you have a table with a date column and an int column, write a query which gives me the interval start date and end date, and the sum of the values for the interval. In other words, if we have these records in table<br /><br /> Dt1, 5<br /> Dt2, 6<br /> Dt3, 2<br /> Dt4, 8<br /> <br /> I want the result<br /> <br /> Dt1, Dt2, 11 (5+6)<br /> Dt2, Dt3, 8 (6 + 2)<br /> Dt3, Dt4 10 (2 + 8)</p>
<p>The problem was not new for me, since I already had to face it in real life. In this new version of Sql Server 2012 we have to functions <a href="http://msdn.microsoft.com/en-us/library/hh213125.aspx" title="Lead (T-SQL)" target="_blank">Lead</a> and <a href="http://msdn.microsoft.com/en-us/library/hh231256.aspx" title="Lag (T-SQL)" target="_blank">Lag</a> that we can use to solve the problem without having to &nbsp;think too much. So my first answer was <em>"Can I assume that I am using a Sql Server 2012?" &nbsp;</em><span>The guy is a MySql guy, so he answered me </span><em>"No. Write a query that we can port to another database without too much effort"</em><span>.&nbsp;</span><span>So, I wrote the query on paper, which consists of a self join combined with the use of &nbsp;row_number() function.</span><br /><br /> The guy looked to the query and didn't recognize the row_number() function. Then he show me a query using his approach with MySql.<br /> <br /> None of us had our laptop, prepared with the tools to show the solution to each other using a real database instance.&nbsp;<span>We could use SQLFiddle to show the solution to each other.</span></p>
<p>BTW, <a href="http://sqlfiddle.com/#!6/b5d9a/2" target="_blank">here</a> is the solution without using Lead and Lag&nbsp;</p>