---
layout: post
template: post
date: 2014-06-23
tags:
- hubot
title: " Hubot security considerations"
slug: /post/89658506788/hubot-security-considerations
description: "Hubot security considerations"
---
<p>&nbsp;I'm currently working in a project that integrates <a href="https://hubot.github.com/" target="_blank">Hubot</a> with an external system. If you don't know anything about Hubot you can read more <a href="https://github.com/github/hubot/tree/master/docs" target="_blank">here</a>. It's definitely a very endearing project commissioned by Github.</p>
<p>In this small post, I would like to make some security considerations that I thought being useful for someone who needs to install Hubot in production</p>
<p><strong>DIE command</strong></p>
<p>The default Hubot installation (the one that is created by hubot --create mybot) exposes a <strong>DIE command</strong>, included in the ping.coffee file in the scripts folder. Any user in the room can invoke it as '<strong>hubot DIE</strong>'. This command just simply terminates the current hubot instance. A possible mitigation for this risk could be just remove the ping.coffee file from the scripts folder or modify it commenting the command. You could also add an if clause to check if the user running the command has a given role.</p>
<p><strong>Show Storage command</strong></p>
<p>Another command available by default is the 's<strong>how storage</strong>' command, included in the storage.coffee file. This command shows all the data stored in brain (well, not all data since it's just showing the brain JSON object at a maximum of a depth 4). This command is available for everyone, s<strong>o be aware of what kind of information you're storing in brain</strong>. If you are going to install custom scripts that store sensitive information in brain, you should consider &nbsp;remove this command or add some restrictions on running it. You could also encrypt the data before saving in brain.</p>
<p><strong>Auth vs Roles scripts</strong></p>
<p>The default Hubot installtions loads two scripts to manage roles in hubot</p>
<ul>
<li><span><strong>auth.coffee</strong> - Only users registered in <strong>HUBOT_AUTH_ADMIN</strong> env variable can manage roles (kind of secure mode). In this mode only the instance admins can do <strong>'hubot user1 has god role'</strong>.&nbsp;</span></li>
<li><span></span><strong>roles.coffee</strong> - Everyone in the room can manage roles (kind of a free mode). In this mode everyone can do <strong>'hubot user1 is god'</strong></li>
</ul>
<p><strong>These modes are mutually exclusive</strong>. If env variable HUBOT_AUTH_ADMIN is defined and has values (comma separated list of admin user IDs) then hubot uses the auth script, otherwise hubot uses the roles script. <strong>Make sure you define the variable HUBOT_AUTH_ADMIN with the list of user IDs that will be the hubot admins</strong>.</p>
<p><strong>HTTP Endpoint secured</strong></p>
<p>You can expose HTTP endpoints inside Hubot which will run hubot commands. Hubot uses internally the <a href="http://expressjs.com/" target="_blank">Express</a> framework. By default, Hubot exposes these endpoints without any kind of security enabled. <strong>We can enable basic authentication by setting the enviroment variables EXPRESS_USER and EXPRESS_PASSWORD. It's strongly&nbsp;recommended&nbsp;to user strong passwords.</strong></p>
<p></p>
<p>Finally I would like to recommend the book <a href="https://leanpub.com/automation-and-monitoring-with-hubot" target="_blank">Automation and Monitoring with Hubot</a> from <a href="http://varaneckas.com/" target="_blank">Tomas Varaneckas</a>. I bought the book when I started to work with Hubot and I have to say that it helped me a lot to enter quickly inside the world of Hubot.</p>
<p></p>
<p></p>