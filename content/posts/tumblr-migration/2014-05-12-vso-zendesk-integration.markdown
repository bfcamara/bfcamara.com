---
layout: post
template: post
date: 2014-05-12
tags:
- alm
- Visual Studio
title: "Using the Zendesk for Visual Studio Online integration"
slug: /post/85562983333/vso-zendesk-integration
description: "Using the Zendesk for Visual Studio Online integration"
---
<p><a href="http://northamerica.msteched.com/" target="_blank">TechEd NA</a>&rsquo;s recent announcements about <a href="http://www.visualstudio.com/" target="_blank">Visual Studio Online</a> (VSO) <a href="http://www.visualstudio.com/integrate/get-started/get-started-rest-basics-vsi" target="_blank">Service Hooks and REST APIs</a>, will make us believe the sky is the limit regarding integrations between VSO and external services and applications.</p>
<p>The <a href="http://www.visualstudio.com/integrate/reference/reference-vso-overview-vsi" target="_blank">REST AP</a>I&nbsp;not only allows to get and manipulate data on VSO from a multi-platform perspective but the <a href="http://www.visualstudio.com/integrate/reference/reference-vso-hooks-overview-vsi" target="_blank">Service Hooks</a> will enable applications to get notifications about changes on VSO in real time.</p>
<p>But I&rsquo;m digressing; this isn&rsquo;t about the APIs nor the service hooks capabilities, but of a particular scenario that has been implemented using these new capabilities.</p>
<p>One of the available service hooks that is available is Zendesk. Using service hooks, paired with a Zendesk app we are now able to reduce the gap between the support team using Zendesk to doing customer care and the development team workflow that is using VSO.</p>
<p>This integration is enabled by using a Zendesk app that allows the support team to better collaborate by escalating tickets to the dev team directly from Zendesk without the need for <a href="http://www.webopedia.com/TERM/S/swivel_chair_interface.html" target="_blank">swivel chair integration</a>.</p>
<p>The app is available as open source in Codeplex&rsquo;s project the <a href="https://vsozendesk.codeplex.com/" target="_blank">Visual Studio Online App for Zendesk</a> and can be installed on your Zendesk account.</p>
<p>For example, when a customer reports a bug, the agent can create a work item in VSO directly from Zendesk. After fixing the bug, a developer can add a comment to the ticket directly from Visual Studio Online. An agent can also notify developers by adding comments to the work items without leaving Zendesk.</p>
<p>The main purpose of this post is to show how to setup and use the Zendesk for Visual Studio Online integration.</p>
<h2><strong>Setup integration</strong></h2>
<p>The setup process consists of:</p>
<ol><li>Installing the Zendesk application in Zendesk</li>
<li>Creating a Zendesk service hook subscription in Visual Studio Online</li>
</ol><p>&nbsp;</p>
<h3><strong>Install application in Zendesk</strong></h3>
<p><span>To install the application you must first download the latest version of the application zip package from </span><a href="https://vsozendesk.codeplex.com/">CodePlex</a><span>. You can install the app in Zendesk by navigating to </span><strong>APPS </strong><strong>&agrave;</strong><strong> Manage</strong><span> and clicking in the </span><strong>Upload App</strong><span> button.</span></p>
<p>&nbsp;<figure class="tmblr-full" data-orig-height="280" data-orig-width="500" data-orig-src="./9b247ec0a87e269f1f4989f4a9b2a1b7da587097e9d07abb1e44d57039d27597.png"><img alt="image" src="./7ce9309ba740e84bd7a4b0e20cd93ef3496e7deffb7022ffe247ad06495e076f.png" data-orig-height="280" data-orig-width="500" data-orig-src="./9b247ec0a87e269f1f4989f4a9b2a1b7da587097e9d07abb1e44d57039d27597.png"></figure></p>
<p></p>
<p>Before starting the installation process, Zendesk requires you accept the license. After the upload is done we will be redirected to the settings page to configure the app:</p>
<ul><li>VSO account name &agrave; VSO account name, like in https://<strong>&lt;account_name&gt;</strong>.visualstudio.com.</li>
<li>VSO work item tag &agrave; the tag that will be applied in VSO to linked work items. This tag will be used in Visual Studio Online when a work item is created directly from Zendesk.</li>
<li>Enable role restrictions? &agrave;select which roles (administrator, agent) have access to the app</li>
</ul><p>The tag is optional, but by defining a tag you can easily find which work items have been escalated from Zendesk with the use of a query. Furthermore, it also allows you to quickly see on the backlog which work items have been originated from Zendesk (by showing the tags columns)</p>
<p>Fill the settings and click <strong>Install</strong>.</p>
<p><figure class="tmblr-full" data-orig-height="545" data-orig-width="500" data-orig-src="./62c69848f23e7f2e3b3b79d65dbb6b9907f0f066d20bc5ef0a90c78638eb8719.png"><img alt="image" src="./f881dcd74abc78c23b8e5fb40f80b673760a28c1c44494bc7ac879c5c1291332.png" data-orig-height="545" data-orig-width="500" data-orig-src="./62c69848f23e7f2e3b3b79d65dbb6b9907f0f066d20bc5ef0a90c78638eb8719.png"></figure></p>
<p></p>
<p>&nbsp;</p>
<p>The app is now installed.&nbsp;</p>
<p><figure class="tmblr-full" data-orig-height="132" data-orig-width="500" data-orig-src="./ebcd1e418445cdf9d8b25abadb3114d1139e43c6528c8a2cde4ed6e50c8ae642.png"><img alt="image" src="./b4537d99a7256495c7f8e5b22b15fdc2ea20a6ecef268b3bfe00efb6cca27bae.png" data-orig-height="132" data-orig-width="500" data-orig-src="./ebcd1e418445cdf9d8b25abadb3114d1139e43c6528c8a2cde4ed6e50c8ae642.png"></figure></p>
<h3><strong>Creating a Zendesk subscription in Visual Studio online</strong></h3>
<p><span>To create a new service hook subscription, in the VSO project portal, we need to go to the a project control panel by clicking the cog icon (any project will do)</span></p>
<p><figure class="tmblr-full" data-orig-height="230" data-orig-width="500" data-orig-src="./393827befc2e7bb55ce5d2e7ae93859a92ceac7837c26cc6d3a66233a3f761ae.png"><img alt="image" src="./65dc5bb51fcfad23e0ef09dd200091b52fea68870db9d37b06a2206565da9ee3.png" data-orig-height="230" data-orig-width="500" data-orig-src="./393827befc2e7bb55ce5d2e7ae93859a92ceac7837c26cc6d3a66233a3f761ae.png"></figure></p>
<p>In the project&rsquo;s control panel, choose the tab Service Hooks</p>
<p><figure class="tmblr-full" data-orig-height="181" data-orig-width="500" data-orig-src="./36f619560b24ae3ad4759032c0fb680e8f83d0776e8eb39282b80bd468e61089.png"><img alt="image" src="./e528a10f48ecdf351d909b2b0a406745f917a31c1f036b4cd580aeefae9e871c.png" data-orig-height="181" data-orig-width="500" data-orig-src="./36f619560b24ae3ad4759032c0fb680e8f83d0776e8eb39282b80bd468e61089.png"></figure></p>
<p>Then, click on the link &ldquo;Create the first subscription for this project&rdquo;</p>
<p><figure class="tmblr-full" data-orig-height="159" data-orig-width="500" data-orig-src="./9191b9295c462591ea848349a3d8ea3c7f9845090e3be1be9afccc73000143ea.png"><img alt="image" src="./6ce3ce6424b9f4af295b85247dcdd4b73a4837a54c5d5e86511e402aeb862aad.png" data-orig-height="159" data-orig-width="500" data-orig-src="./9191b9295c462591ea848349a3d8ea3c7f9845090e3be1be9afccc73000143ea.png"></figure></p>
<p></p>
<p>Start by creating a new service hook subscription by choosing Zendesk as the target service</p>
<p><figure class="tmblr-full" data-orig-height="498" data-orig-width="500" data-orig-src="./dde89d4e968738dbb0b5a162cdc53d797b28154e71af19b91599f6ac8484dcb8.png"><img alt="image" src="./cdffa8b05619765c79669490b537ce30815e97d42572887fdb33bd40de0bee2c.png" data-orig-height="498" data-orig-width="500" data-orig-src="./dde89d4e968738dbb0b5a162cdc53d797b28154e71af19b91599f6ac8484dcb8.png"></figure></p>
<p>The Zendesk service hook provides just one action to create a private comment in a Zendesk ticket. This action only supports the event &ldquo;Work item is commented on&rdquo;, and before creating a private comment in Zendesk, it performs additional checks:</p>
<ul><li><strong>The comment should start with the pattern defined in the field &ldquo;Contains only&rdquo;</strong>. When defining a subscription to events &ldquo;Work item is commented on&rdquo;, one required parameter is the &ldquo;Contains string&rdquo; field.</li>
</ul><p><figure class="tmblr-full" data-orig-height="402" data-orig-width="500" data-orig-src="./82038b2ef25ad3cb3e84f0ec5116814abd7988bf04921b1a00a4a0e6d6b99e01.png"><img alt="image" src="./7e83f95d616cc5ca031f8ee2545608f4475ab2a971bb85579ff2199b25e5e8ca.png" data-orig-height="402" data-orig-width="500" data-orig-src="./82038b2ef25ad3cb3e84f0ec5116814abd7988bf04921b1a00a4a0e6d6b99e01.png"></figure>In other words, only comments that start with the pattern (In this example <em>Zendesk:</em>) will be considered by the subscription. For example</p>
<p><figure class="tmblr-full" data-orig-height="148" data-orig-width="500" data-orig-src="./753496f96e096307386c78b0c6f7f01eba8087ee1101612d99a284572fb2d6f9.png"><img alt="image" src="./bfb69cdb0893dcb74ad0809a5daf8f1e7f437db7d8c11d85303b2a973b92c8b1.png" data-orig-height="148" data-orig-width="500" data-orig-src="./753496f96e096307386c78b0c6f7f01eba8087ee1101612d99a284572fb2d6f9.png"></figure></p>
<ul><li><strong>The comment should be related with a Zendesk ticket</strong>. The relation is defined using hyperlinks, which means that a given work item is related to a Zendesk ticket if it has in its links a hyperlink to the URL of the ticket details page in Zendesk agent app. (don&rsquo;t worry this relation is created automatically by the app when the work item is created on VSO, no manual intervention is needed)</li>
</ul><p><figure class="tmblr-full" data-orig-height="136" data-orig-width="362" data-orig-src="./719c3eaaeeb3239f5670c7bae4fe1de9bdcc3829454d78c09493dc673dc47b88.png"><img alt="image" src="./bab91418679074b1daa3bbac9a35519bc952e6842103ffe3d4f479da47c67ceb.png" data-orig-height="136" data-orig-width="362" data-orig-src="./719c3eaaeeb3239f5670c7bae4fe1de9bdcc3829454d78c09493dc673dc47b88.png"></figure></p>
<p></p>
<p>When registering the subscription, the following Zendesk inputs will be asked:</p>
<ul><li>The Zendesk account name</li>
<li>User name</li>
<li>API token</li>
</ul><p><figure class="tmblr-full" data-orig-height="356" data-orig-width="413" data-orig-src="./2f3bffe0e669adbac9001f828f72d5680da0efc3e1c1a46a5843649dae5883d5.png"><img alt="image" src="./eb2dfd65b0ce30134b793dad0be5e4bbc1e2264367db7711e2807595271850de.png" data-orig-height="356" data-orig-width="413" data-orig-src="./2f3bffe0e669adbac9001f828f72d5680da0efc3e1c1a46a5843649dae5883d5.png"></figure></p>
<h4><strong>Zendesk account name</strong></h4>
<p>The Zendesk account name is the subdomain part of the URL used to access the Zendesk account like in https://<strong>&lt;account_name&gt;</strong>.zendesk.com</p>
<h4><strong>Zendesk user name</strong></h4>
<p>The Zendesk user name is the email address of the agent that will be used to perform the integration (this can be seen as the integration account which will be used to perform operations on Zendesk). The list of available users can be viewed in People menu option in the Manage group.</p>
<p>&nbsp;<figure class="tmblr-full" data-orig-height="301" data-orig-width="500" data-orig-src="./23bb69b5975afe2409d5d9c085e126a6dcbaa90dbd003191d3d4c909f31bbf55.png"><img alt="image" src="./31e9f69b1926925427d577f66617a5f3fb219aa92354af4bb21d33b71592767b.png" data-orig-height="301" data-orig-width="500" data-orig-src="./23bb69b5975afe2409d5d9c085e126a6dcbaa90dbd003191d3d4c909f31bbf55.png"></figure></p>
<h4><strong>API token</strong></h4>
<p>The API token is the Zendesk API authentication token for the agent who will perform the integration. To be able to use API tokens a user must explicitly enable access with tokens, which can be done using the API menu option inside the Channels group</p>
<p><figure class="tmblr-full" data-orig-height="300" data-orig-width="500" data-orig-src="./667f391741a423686c8ebb064764806c2ddbd8f23990a597d0567827d3ae7280.png"><img alt="image" src="./e19bc0eee0d2e6b5f5e3bc766844e74c3c27afc781f7aa85bddbfc050d9207cc.png" data-orig-height="300" data-orig-width="500" data-orig-src="./667f391741a423686c8ebb064764806c2ddbd8f23990a597d0567827d3ae7280.png"></figure></p>
<p></p>
<p>We have now a Zendesk service hook subscription created</p>
<p><figure class="tmblr-full" data-orig-height="159" data-orig-width="500" data-orig-src="./67ef5e51c25f87b839b0b88446c86c8eae5e51af36d1d05d424a5121cdecffd5.png"><img alt="image" src="./1735fc46020a510598c0f07938e55373672b34e48524b2eb881f66785bf39819.png" data-orig-height="159" data-orig-width="500" data-orig-src="./67ef5e51c25f87b839b0b88446c86c8eae5e51af36d1d05d424a5121cdecffd5.png"></figure></p>
<h2><strong>Using the Zendesk application</strong></h2>
<p>The VSO App for Zendesk is only activated in a Ticket side bar. Let&rsquo;s start by creating a new ticket and submit it.</p>
<p><figure class="tmblr-full" data-orig-height="320" data-orig-width="500" data-orig-src="./7469c72a5b020300fef905c46ae57aedbc1fccdd764f6dcf2c8ba53eebef3ea7.png"><img alt="image" src="./57473c3768710af72a2b5f68f5455df1e6b12b88ec9f3e1659ccf22a99ae3368.png" data-orig-height="320" data-orig-width="500" data-orig-src="./7469c72a5b020300fef905c46ae57aedbc1fccdd764f6dcf2c8ba53eebef3ea7.png"></figure></p>
<p>Go to the details page of the ticket just created and you should see the app in the sidebar. The first time the app is loaded it will ask the agent for his VSO alternate credentials (this gives you full auditability capabilities since the work item will be created in VSO with the agent&rsquo;s account).</p>
<p>If you don&rsquo;t know how to enable/set alternate credentials you can read more about it on <a href="http://blogs.msdn.com/b/buckh/archive/2013/01/07/how-to-connect-to-tf-service-without-a-prompt-for-liveid-credentials.aspx" target="_blank">this Buck Hodges blog post</a></p>
<p><figure class="tmblr-full" data-orig-height="206" data-orig-width="500" data-orig-src="./5b229f53b323953a729fa8ecd36dbe4f70ed0e437978693281b60747c6ac3806.png"><img alt="image" src="./ae14833bc87e9399c9c254916178c0034416b84bfe550867f4b57dd90914b1b8.png" data-orig-height="206" data-orig-width="500" data-orig-src="./5b229f53b323953a729fa8ecd36dbe4f70ed0e437978693281b60747c6ac3806.png"></figure></p>
<p></p>
<p>Fill in the agent Visual Studio Online alternate credentials and click <strong>Login</strong>.</p>
<p><figure class="tmblr-full" data-orig-height="403" data-orig-width="472" data-orig-src="./cfe963931ded2b646177023d966b33ed96c7c6a5e808e0fb4a8091f4beba1983.png"><img alt="image" src="./7beee5217dda121925ed6966972d3d91f9f41bd32fe539a40cbe93940bc4c02e.png" data-orig-height="403" data-orig-width="472" data-orig-src="./cfe963931ded2b646177023d966b33ed96c7c6a5e808e0fb4a8091f4beba1983.png"></figure></p>
<p>You should get a success notification.</p>
<p><figure class="tmblr-full" data-orig-height="296" data-orig-width="420" data-orig-src="./1308897af648f9023681d93724b04ddf91354c6a42d34adb3529ec19025d3b1e.png"><img alt="image" src="./98ba9c36807518e4ea8bbff6184220effc16fd876fdb50d8bbed42854e86d3fa.png" data-orig-height="296" data-orig-width="420" data-orig-src="./1308897af648f9023681d93724b04ddf91354c6a42d34adb3529ec19025d3b1e.png"></figure></p>
<p></p>
<p>Since the ticket is not yet associated with a work item, you obviously don&rsquo;t see the work item, but you can now see the actions that will allow you to create a work item associated with the ticket.</p>
<p></p>
<h3><strong>New work item</strong></h3>
<p>Now let&rsquo;s create a work item from a ticket. In the ticket details page, click in <strong>New work item</strong> button.</p>
<p><figure class="tmblr-full" data-orig-height="188" data-orig-width="500" data-orig-src="./ccfa4ac6c244215121d34229676a026fa57042dff34a51ae8383e6c705766200.png"><img alt="image" src="./09fe759081a7c1ce5217ae04415c9dbf3c108daf668e70ade761ea38e218048d.png" data-orig-height="188" data-orig-width="500" data-orig-src="./ccfa4ac6c244215121d34229676a026fa57042dff34a51ae8383e6c705766200.png"></figure></p>
<p>The app will open a modal dialog in a loading state while is fetching some data from VSO, and then will show the following form</p>
<p><figure class="tmblr-full" data-orig-height="336" data-orig-width="500" data-orig-src="./bf7cb3ebbef4d4aa30a00ed5da9f04b7e819a4c50b9e7642a99067f6aac637b3.png"><img alt="image" src="./a60120ea64a900ccf7e6cc537dbcdb3a1022a759014516812205de193a6dcc98.png" data-orig-height="336" data-orig-width="500" data-orig-src="./bf7cb3ebbef4d4aa30a00ed5da9f04b7e819a4c50b9e7642a99067f6aac637b3.png"></figure></p>
<p>Fill in the form fields and click <strong>Create work item</strong> button (the fields should be self-explanatory).</p>
<p>Notice you can choose the work item type; the available work item types depend on the process template where the work item is being created.</p>
<p>The summary is pre filled from the ticket title, but you can change it if you want.</p>
<p>The description is not pre filled, but you can copy the ticket description, by clicking on the link &ldquo;Copy the ticket&rsquo;s description&rdquo;</p>
<p><figure class="tmblr-full" data-orig-height="319" data-orig-width="500" data-orig-src="./4fa35df2728e3ce1dda260dd2f9177f1266e3237d789efdc02f603238bd16697.png"><img alt="image" src="./7c42fa66d190be95f5182e4d6f8dc824fe35219480dfd8067cc468085fe479fc.png" data-orig-height="319" data-orig-width="500" data-orig-src="./4fa35df2728e3ce1dda260dd2f9177f1266e3237d789efdc02f603238bd16697.png"></figure></p>
<p>While the operation is being performed you should see a waiting spinner, and when completed, you will be notified and the work item is now visible as being linked</p>
<p><figure class="tmblr-full" data-orig-height="484" data-orig-width="383" data-orig-src="./e0340f57d3565ccff6135d6a5bccb9d9534db823d44eae873ad1aa7ab0b85fa5.png"><img alt="image" src="./01d7bfd9e40bb93fa3b01f043a9483764dd294fbe0b9c27da7c29cf27a8cc534.png" data-orig-height="484" data-orig-width="383" data-orig-src="./e0340f57d3565ccff6135d6a5bccb9d9534db823d44eae873ad1aa7ab0b85fa5.png"></figure></p>
<p>If you click on the work item title in the sidebar, a new window will be opened in VSO with the work item details. Click on it to the work item in VSO side. You will be able to see how the work item has been created correctly (Id, title, description, work item type, tag and link).</p>
<p><figure class="tmblr-full" data-orig-height="273" data-orig-width="500" data-orig-src="./a0cd0e6aaebe3b686a9580c26b1c91ded503faf49a55762a3896f533909878db.png"><img alt="image" src="./14ba4dca1fbc9ce9492197411247e7107b77d9bacad49d10e9102e410707756d.png" data-orig-height="273" data-orig-width="500" data-orig-src="./a0cd0e6aaebe3b686a9580c26b1c91ded503faf49a55762a3896f533909878db.png"></figure></p>
<p>Notice the link to the ticket on the Links tab. This not only allows the Service Hook to determine the association between the work item and the ticket, but it will easily allow the developers to go directly to the ticket Zendesk if they wish too.</p>
<p>Remember, if you delete this link comments made on the work item (using the keyword) will NOT be propagated to Zendesk.</p>
<p>&nbsp;</p>
<h3><strong>Comment work item in Visual Studio Online</strong></h3>
<p>For the work item just created, let&rsquo;s make a comment in Visual Studio Online</p>
<p>&nbsp;<figure class="tmblr-full" data-orig-height="146" data-orig-width="500" data-orig-src="./ffb49b72b254c45a532912ad43ad3a3a3d750d9b0d3a8489619239b3504ed243.png"><img alt="image" src="./e70cf01aa23016dfbafb7894e56f6eff54c41fc01430dd67fa17c68a6507b7b1.png" data-orig-height="146" data-orig-width="500" data-orig-src="./ffb49b72b254c45a532912ad43ad3a3a3d750d9b0d3a8489619239b3504ed243.png"></figure></p>
<p></p>
<p></p>
<p>Notice that we started our comment with <strong>Zendesk:</strong> which means that the subscription will integrate this comment in Zendesk (this was previously configured in the Service Hook configuration, if you used a different pattern then use it accordingly). Go to your Zendesk app and open the ticket, you can now check the new comment is there</p>
<p><figure class="tmblr-full" data-orig-height="168" data-orig-width="500" data-orig-src="./e205bc709fb5bf36271ed7992488afd1e705593ead9d3834754e920722d86499.png"><img alt="image" src="./48cc5b4158c0e5abcfd5e5a6cced4b0784f379c80643c7b0d630955922d5b735.png" data-orig-height="168" data-orig-width="500" data-orig-src="./e205bc709fb5bf36271ed7992488afd1e705593ead9d3834754e920722d86499.png"></figure></p>
<h3><strong>View work item details</strong></h3>
<p>&nbsp;You can view the work item by click on <strong>Eye icon</strong>.</p>
<p><figure class="tmblr-full" data-orig-height="396" data-orig-width="372" data-orig-src="./8b774aae603bd324504990506955525a77d8e9de54e2c520eb5c9c2a3585d930.png"><img alt="image" src="./c8c7c6e1acee41828c0a1dba858af249ad57d6f9ac138f06dc579406d764d4a0.png" data-orig-height="396" data-orig-width="372" data-orig-src="./8b774aae603bd324504990506955525a77d8e9de54e2c520eb5c9c2a3585d930.png"></figure></p>
<p></p>
<p>This will pop up a modal window that will allow you to see a work item a detailed view without leaving Zendesk</p>
<p><figure class="tmblr-full" data-orig-height="281" data-orig-width="500" data-orig-src="./439eb95021a2716b558f306466237075c6bb842292403b5cc2ed5cd34f728b5f.png"><img alt="image" src="./ba8de716042da494e24f4073e13697d171ed45e588ed196e1d251e2eab71b28c.png" data-orig-height="281" data-orig-width="500" data-orig-src="./439eb95021a2716b558f306466237075c6bb842292403b5cc2ed5cd34f728b5f.png"></figure></p>
<p>You can customize the fields visible in the summary view and the detailed view by clicking on the <strong>Cog icon</strong>.</p>
<p><figure class="tmblr-full" data-orig-height="484" data-orig-width="390" data-orig-src="./efc0d9d81e34dbdfc0e68b34ad414ffc0e36d8be7657ee452c8effcab2ec150f.png"><img alt="image" src="./14e3412d5df117864431663b2a8d71c043df0880bfbc9c419d8f7cd7230dc79b.png" data-orig-height="484" data-orig-width="390" data-orig-src="./efc0d9d81e34dbdfc0e68b34ad414ffc0e36d8be7657ee452c8effcab2ec150f.png"></figure></p>
<p>You should see the following page</p>
<p><figure class="tmblr-full" data-orig-height="734" data-orig-width="446" data-orig-src="./d6382a8121b0d505332b54b19c1a5241a5d18938d9f99669b8a7ea2fd116b5f4.png"><img alt="image" src="./e5614585cc29a8022da2f16c70300ea118716bae9fc6a760eb037e63693a6efd.png" data-orig-height="734" data-orig-width="446" data-orig-src="./d6382a8121b0d505332b54b19c1a5241a5d18938d9f99669b8a7ea2fd116b5f4.png"></figure></p>
<p></p>
<p>Here we can choose the fields shown in the summary and the details page. As you start changing your selection, the settings are automatically saved. Let&rsquo;s add the State to the Summary page and the State and the Severity to Details page.</p>
<p><figure class="tmblr-full" data-orig-height="744" data-orig-width="393" data-orig-src="./28c5f48f8e745dcffa9fc94cda548d7c6e7c1ed70ce8e4b3ca2c0b556b14b2d4.png"><img alt="image" src="./960789ba15de15b25d747b2d5418e1204941b371bddc15e3808adddb58d16925.png" data-orig-height="744" data-orig-width="393" data-orig-src="./28c5f48f8e745dcffa9fc94cda548d7c6e7c1ed70ce8e4b3ca2c0b556b14b2d4.png"></figure></p>
<p>And now let&rsquo;s return to the summary page, clicking on the back button.</p>
<p></p>
<p><figure class="tmblr-full" data-orig-height="197" data-orig-width="412" data-orig-src="./7404b1c34cc94e75f19fac095b29d31f61cd1eb9d300b929a099be3ced90e927.png"><img alt="image" src="./cd6ab04f16a177b70b78242203dbeefb17462c514d31ece71b4d7da15c11be15.png" data-orig-height="197" data-orig-width="412" data-orig-src="./7404b1c34cc94e75f19fac095b29d31f61cd1eb9d300b929a099be3ced90e927.png"></figure></p>
<p></p>
<p>You should now see the State field</p>
<p><figure class="tmblr-full" data-orig-height="394" data-orig-width="392" data-orig-src="./4de1c9e4d29b8f57e7c0e3b375b3289cd65dd0c6f09f29c4f9128d0a669ff1dc.png"><img alt="image" src="./857fd0476436ad5138ed5c1610341eac1de0df4630c722988063180e11bae437.png" data-orig-height="394" data-orig-width="392" data-orig-src="./4de1c9e4d29b8f57e7c0e3b375b3289cd65dd0c6f09f29c4f9128d0a669ff1dc.png"></figure></p>
<p>And click again in the eye icon to check the details page and see the State and Severity fields.</p>
<p><figure class="tmblr-full" data-orig-height="354" data-orig-width="500" data-orig-src="./a06302a6328dc94cfb9e36f829efb9f72302fbfe8f8684b37206d0a2078c8776.png"><img alt="image" src="./55334dfd5b2fa0f2d686d104c14af55de2c9dada082982281133da0d3fab0d4b.png" data-orig-height="354" data-orig-width="500" data-orig-src="./a06302a6328dc94cfb9e36f829efb9f72302fbfe8f8684b37206d0a2078c8776.png"></figure></p>
<p>To summarize, the summary view is what you get in the sidebar and the detail view is what you view (in a modal window) if you want to see a more detailed view of the work item. This allows you to show the most relevant fields to your agents without the need to ever leave Zendesk</p>
<h3><strong>Link to work item</strong></h3>
<p>Instead of creating a new work item, we can choose to link the ticket to an existent work item. Click on button <strong>Link to work item</strong>.</p>
<p><figure class="tmblr-full" data-orig-height="194" data-orig-width="500" data-orig-src="./e3c7b370abcd69988d9b73349401d57c6682a6d5d04625e3620fcec508a4f3e3.png"><img alt="image" src="./505cd530d447828bf2efa5a7a333b95f9f858fa10d32828d14eca21b93f4b109.png" data-orig-height="194" data-orig-width="500" data-orig-src="./e3c7b370abcd69988d9b73349401d57c6682a6d5d04625e3620fcec508a4f3e3.png"></figure></p>
<p>You should see the dialog below</p>
<p><figure class="tmblr-full" data-orig-height="218" data-orig-width="500" data-orig-src="./02090d2663b8f53f9c2e19da38042c31d90ed5d9d8e164616ea4146f4177c7e1.png"><img alt="image" src="./86059f659eb58ba80b4f6fd0dab6bcd314c6741484059007b3336e9c27e9d32e.png" data-orig-height="218" data-orig-width="500" data-orig-src="./02090d2663b8f53f9c2e19da38042c31d90ed5d9d8e164616ea4146f4177c7e1.png"></figure></p>
<p>If you know the work item number, just enter it in the Work item text box and click Link. Otherwise you can click <strong>Search</strong> to execute a VSO work item query.</p>
<p>I will notice you can now see new section called Search that allows you to select the team project and the query to execute (this is the list of queries stored on the team project)</p>
<p><figure class="tmblr-full" data-orig-height="265" data-orig-width="500" data-orig-src="./df63a770ba4bbc37c84a3f87c1c268c90b19fd79cbb25491181b2f5e74c54c1e.png"><img alt="image" src="./8f0c53d640e8f453c448a277ea18d114949bc8602a43532d747a495ccc94233f.png" data-orig-height="265" data-orig-width="500" data-orig-src="./df63a770ba4bbc37c84a3f87c1c268c90b19fd79cbb25491181b2f5e74c54c1e.png"></figure></p>
<p>Choose the query you want to execute and click on <strong>Query</strong>.&nbsp;</p>
<p><figure class="tmblr-full" data-orig-height="421" data-orig-width="500" data-orig-src="./9d0ec8407af02486c1f51224a7dbc0e60e27406bf0b826d4b21387c56c1af7bb.png"><img alt="image" src="./8383bdf1bdb2e40dc823b272efcfde861671c211eb07ef635e0a97c1951fb05c.png" data-orig-height="421" data-orig-width="500" data-orig-src="./9d0ec8407af02486c1f51224a7dbc0e60e27406bf0b826d4b21387c56c1af7bb.png"></figure></p>
<p>Click the Link icon of the row you want to select and its work item id should be copied to the text box. Now you can click the Link button.</p>
<p><figure class="tmblr-full" data-orig-height="196" data-orig-width="500" data-orig-src="./6ea5548eef787d55eb24012a6b9a15f7d7a075735b3822df289e0eec05ada926.png"><img alt="image" src="./84be8eaba4dc8b1896f3b08cb879cf9f019837d3952b66f2f7697e3aa8f979b4.png" data-orig-height="196" data-orig-width="500" data-orig-src="./6ea5548eef787d55eb24012a6b9a15f7d7a075735b3822df289e0eec05ada926.png"></figure></p>
<p>While performing the operation you should see a spinner loading, when the operation is completed you should be notified and see the work item in the list of linked work items.</p>
<p><figure class="tmblr-full" data-orig-height="544" data-orig-width="413" data-orig-src="./162fbe19e9674cda061657d81c03bd07725d88509076d7b35273e1c3dc8f7709.png"><img alt="image" src="./ab678393aa2c00cd4c2a60eaec0a640c714552a5a82eb678fbbe05d3f171f1a1.png" data-orig-height="544" data-orig-width="413" data-orig-src="./162fbe19e9674cda061657d81c03bd07725d88509076d7b35273e1c3dc8f7709.png"></figure></p>
<p>Let&rsquo;s check how the work item was updated in VSO. As a result of the linking process, the work item should have been updated with a hyperlink with the ticket URL.</p>
<p><figure class="tmblr-full" data-orig-height="226" data-orig-width="500" data-orig-src="./8d8d1639d83718bb8b0be106c491bdd7bdeb0ff59f80e54c7bfb5cbb2d69cb0d.png"><img alt="image" src="./e1a447a7307a6d68c64c11d129eb3839ac66ec82ed077e76721d79299a0f540b.png" data-orig-height="226" data-orig-width="500" data-orig-src="./8d8d1639d83718bb8b0be106c491bdd7bdeb0ff59f80e54c7bfb5cbb2d69cb0d.png"></figure></p>
<p></p>
<h3><strong>Notify</strong></h3>
<p>A Zendesk user can send a notification to all the linked work items. This will result in the addition of a comment to all linked work item(s). To notify linked work items click on <strong>Notify</strong> button.</p>
<p><figure class="tmblr-full" data-orig-height="493" data-orig-width="412" data-orig-src="./a7b0da9ffc891d0e606e569031c3e329b65e8f09d56749da3474c88a9460ff92.png"><img alt="image" src="./4e6621748c3265b14b30f165495e9e9237cbcdf47deb354315bbce0ce361dce3.png" data-orig-height="493" data-orig-width="412" data-orig-src="./a7b0da9ffc891d0e606e569031c3e329b65e8f09d56749da3474c88a9460ff92.png"></figure></p>
<p>You should see the following dialog</p>
<p></p>
<p><figure class="tmblr-full" data-orig-height="259" data-orig-width="500" data-orig-src="./b37e684cce2c1e32bf9b7d26af66ab29b7363bac65a7b5ba416be96e2bffee59.png"><img alt="image" src="./6a4ae483a61c0ee4df9f93f19286f6ff05c3f962a91bc38b58d3403aaec031d7.png" data-orig-height="259" data-orig-width="500" data-orig-src="./b37e684cce2c1e32bf9b7d26af66ab29b7363bac65a7b5ba416be96e2bffee59.png"></figure></p>
<p></p>
<p></p>
<p>Fill in the notification and click <strong>Notify work items</strong>.</p>
<p><figure class="tmblr-full" data-orig-height="246" data-orig-width="500" data-orig-src="./84267dc2193768c3e8e7780fda81067ea54b60a23305d2873275b832300fa960.png"><img alt="image" src="./ecf9d6f7521a1a8f1613c139544ed881900b0a0f118d50586d3c2d3ee23eb3c4.png" data-orig-height="246" data-orig-width="500" data-orig-src="./84267dc2193768c3e8e7780fda81067ea54b60a23305d2873275b832300fa960.png"></figure></p>
<p><figure class="tmblr-full" data-orig-height="534" data-orig-width="396" data-orig-src="./b0c2823102cbace5f59c1912e10c05327c1159610e8490a5101e14cceb6c164f.png"><img alt="image" src="./26c4493614090d94046ae3086b3b8817e54f90e537d122418a93946ac79dc3ca.png" data-orig-height="534" data-orig-width="396" data-orig-src="./b0c2823102cbace5f59c1912e10c05327c1159610e8490a5101e14cceb6c164f.png"></figure></p>
<p>We can now check on VSO the new added comment.</p>
<p><figure class="tmblr-full" data-orig-height="264" data-orig-width="500" data-orig-src="./a5f173e2f38960eeb6f2d64bc012c227dd8d19a53f80b0cd179379be75c8d7a6.png"><img alt="image" src="./214943d83063e473877851ea7305c4375983d7e57e0ac1195994888643c9f4f2.png" data-orig-height="264" data-orig-width="500" data-orig-src="./a5f173e2f38960eeb6f2d64bc012c227dd8d19a53f80b0cd179379be75c8d7a6.png"></figure></p>
<p></p>
<p><figure class="tmblr-full" data-orig-height="257" data-orig-width="500" data-orig-src="./13973edbf709441482f0553a25bdac9afe3ccd5655c0424cf3b48e72293608ae.png"><img alt="image" src="./d496596b38aa70a1b3c21215080edb1e61c27fedf73195fa864d2e282b3c164a.png" data-orig-height="257" data-orig-width="500" data-orig-src="./13973edbf709441482f0553a25bdac9afe3ccd5655c0424cf3b48e72293608ae.png"></figure></p>
<p></p>
<p></p>
<h3><strong>Unlink work item</strong></h3>
<p>&nbsp;You can also unlink a linked work item, by clicking on the Cross icon.</p>
<p><figure class="tmblr-full" data-orig-height="466" data-orig-width="387" data-orig-src="./334ee311a4bcbc24065dc76888720858c97fa6784c7c58b3f59aa521d968b303.png"><img alt="image" src="./83b0d563ce28258673ad407547d26a47f8d9580bb5102c49c38896dd80f0d30d.png" data-orig-height="466" data-orig-width="387" data-orig-src="./334ee311a4bcbc24065dc76888720858c97fa6784c7c58b3f59aa521d968b303.png"></figure></p>
<p>You need to confirm the operation.</p>
<p><figure class="tmblr-full" data-orig-height="188" data-orig-width="500" data-orig-src="./ef5637ad427bfb9a9a0825ae99c10fc6471f0278bbf6aea3011260f73cc04553.png"><img alt="image" src="./55a597d9ade1262670eee4818262e1ab21660fb126d8ac496a9ad21112a1e856.png" data-orig-height="188" data-orig-width="500" data-orig-src="./ef5637ad427bfb9a9a0825ae99c10fc6471f0278bbf6aea3011260f73cc04553.png"></figure></p>
<p>Click &lsquo;Yes, unlink it&rsquo; and you should see a spinner while performing the action, you will be notified when the operation is completed.</p>
<p><figure class="tmblr-full" data-orig-height="459" data-orig-width="376" data-orig-src="./d172a3ee4ab4a53f45c0cda4d9c7b852033a2a731f8db28573ddd41d4cf3ffc4.png"><img alt="image" src="./03c6147f8d18748d02700404eccddf6915d241fd5b666ff0e62c865155dafa2a.png" data-orig-height="459" data-orig-width="376" data-orig-src="./d172a3ee4ab4a53f45c0cda4d9c7b852033a2a731f8db28573ddd41d4cf3ffc4.png"></figure></p>
<p>If you now see the work item in VSO, you will notice the tag and the link to the ticket have been removed.</p>
<p><figure class="tmblr-full" data-orig-height="265" data-orig-width="500" data-orig-src="./a1b9615150adad25e55b9bb8d739f710f50257fc5bbc6c21c502c1e4816b860a.png"><img alt="image" src="./500f4daa7bbe0d8058acbfcafd1df80915445b265ec22b2bbf5fa335f0791f7e.png" data-orig-height="265" data-orig-width="500" data-orig-src="./a1b9615150adad25e55b9bb8d739f710f50257fc5bbc6c21c502c1e4816b860a.png"></figure></p>
<h2><strong>Conclusion</strong></h2>
<p><span>I believe this is a great example of what possibilities can be achieved with Visual Studio Online REST APIs and service hook integrations.</span></p>
<p>The Zendesk for Visual Studio Online application is an example of an implementation and a scenario, in which the collaboration between the development team and the support team is increased without going to great hoops, each using their own tools.</p>
<p>I&rsquo;m really curious what kind of other scenarios people will implement with these new VSO functionalities.</p>
<p></p>
<p></p>
<p></p>
<p></p>