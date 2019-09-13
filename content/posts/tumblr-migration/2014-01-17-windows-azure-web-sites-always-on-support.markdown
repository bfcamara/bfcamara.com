---
layout: post
template: post
date: 2014-01-17
tags:
- azure
title: "Link post"
slug: /post/73559159580/windows-azure-web-sites-always-on-support
description: "Windows Azure: Web Sites Always On Support"
---
<http://weblogs.asp.net/scottgu/archive/2014/01/16/windows-azure-staging-publishing-support-for-web-sites-monitoring-improvements-hyper-v-recovery-manager-ga-and-pci-compliance.aspx>

<blockquote class="link_og_blockquote">
<p><em>One of the other useful Web Site features that we are introducing today is a feature we call &ldquo;Always On&rdquo;.&nbsp; When Always On is enabled on a site, Windows Azure will automatically ping your Web Site regularly to ensure that the<strong> Web Site is always active and in a warm/running state</strong>.&nbsp; This is useful to ensure that a site is always responsive (and that the app domain or worker process has not paged out due to lack of external HTTP requests).&nbsp;</em></p>
<p><em><strong>It also useful as a way to keep a Web Site active for scenarios where you want to run background code</strong> within it irrespective of whether it is actively processing external HTTP customer requests.&nbsp; We have another new feature <strong>we are enabling this week called &ldquo;Web Jobs&rdquo; that makes it really easy to now write this background code and run it within a Web Site</strong>. I&rsquo;ll blog more about this feature and how to use it in the next few days.</em></p>
</blockquote>
<p>Great news (from <a href="http://weblogs.asp.net/scottgu" target="_blank">Scott Guthrie's blog</a>). Until now, my typical approach has been to use <a href="http://www.quartz-scheduler.net/" target="_blank">Quartz.Net</a> to have background jobs, and use a service to ping the web site continuously, using for example <a href="https://www.setcronjob.com/" target="_blank">SetCronJob</a>. From now on, Web Sites have built-in support for Always On, and very soon will have Web Jobs. Well done Azure.</p>
<p></p>