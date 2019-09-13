---
layout: post
template: post
date: 2013-12-13
tags:
- ef
- entity framework
- asp.net mvc
- asp.net
- dotnet
title: "Entity Framework: Resetting your migrations to an initial migration"
slug: /post/69880211351/entity-framework-resetting-your-migrations-to-an
description: "Entity Framework: Resetting your migrations to an initial migration"
---
<p>When using Entity Framework Migrations, probably, <strong>we are going to have too many migrations, even before we have an </strong><strong>initial release&nbsp;</strong>of what we are building (I'm not using Automatic Migrations). <a href="http://www.bfcamara.com/post/68169174771/entity-framework-database-first-vs-code-first">In one of my previous posts I mentioned this problem around Migrations</a>.Let me show you an example of what I'm talking about.&nbsp;</p>
<p><figure class="tmblr-full" data-orig-height="516" data-orig-width="500" data-orig-src="./06990a4b2d108b9743c1d020d7641843840ecfb197b4f4ac669f5e3a6132886b.png"><img alt="image" src="./ebc74bba8a3bfc6aed784ae15763d289befa7fa007964a807b793e7e7fa3275f.png" data-orig-height="516" data-orig-width="500" data-orig-src="./06990a4b2d108b9743c1d020d7641843840ecfb197b4f4ac669f5e3a6132886b.png"></figure></p>
<p></p>
<p></p>
<p>This is the current list of migrations generated so far for a project I am involved. It's a product that is currently in Beta, and we already have deployed it in a testing environment, where we and our beta testers are performing tests.</p>
<p><figure class="tmblr-full" data-orig-height="530" data-orig-width="500" data-orig-src="./7ed34168df6f0158b8917071b3bf44876c111817080adb4de3610515cdd2d4d7.png"><img alt="image" src="./fe5240b6542914a6a7dfe0475ba32f3b011f76c7995e69a8ba292116a750c699.png" data-orig-height="530" data-orig-width="500" data-orig-src="./7ed34168df6f0158b8917071b3bf44876c111817080adb4de3610515cdd2d4d7.png"></figure></p>
<p></p>
<p></p>
<p>I don't want to go into the details of this table, and particularly, how it's used internally by EF, but assume that it is the table that EF uses to know what migrations need to be applied when exists some changes in model.</p>
<p><strong>We are currently at the point where we need to release and prepare a fresh and clean installation in a new environment, but I don't want to have this myriad of migrations, because, in fact, this is my first release</strong> and I don't care about the migrations, and how I got this current model, at least in terms of migrations registration. Of course that I do care about knowing how we got the current model version, and for that we have everything registered in source control.</p>
<p>What I really want is:</p>
<ol><li><strong>to exclude all current migrations from the VS project&nbsp;</strong></li>
<li><strong>to generate again an initial create migration with the current model definition</strong></li>
<li><strong>to have, in my new installation, just one entry in table __MigrationHistory</strong></li>
<li><strong>do not compromise an ugrade to the current test environment</strong></li>
</ol><p></p>
<p><strong>1 - Exclude all migrations</strong></p>
<p>This is a no brain step. <strong>Exclude all the migrations from the VS project</strong>. You just need to have some caution if you have some custom code in migrations, such as for example custom indexes creation. In this case, you need to identify these extra actions which will be used again later.</p>
<p><strong>2 - Generate an Initial Create Migration with all the model definition</strong></p>
<p>Now that we have no migrations in our VS project, we can generate a new migration with all the model definition. First, <strong>we need to create an empty database and change the connectionstring in the web.config to use this new empty database.</strong></p>
<p><span>Then, in Package Manager Console, we <strong>generate a new migration called InitialCreate</strong></span></p>
<p><figure class="tmblr-full" data-orig-height="167" data-orig-width="500" data-orig-src="./33d27fca240b1728b28fb6cfe0c8a42264cc032b44685cf69b5a9b0e72c1b77a.png"><img alt="image" src="./f4717680a4d93917032572b4756b2285c413bfa588adce85b198ff594c1d9f7c.png" data-orig-height="167" data-orig-width="500" data-orig-src="./33d27fca240b1728b28fb6cfe0c8a42264cc032b44685cf69b5a9b0e72c1b77a.png"></figure></p>
<p></p>
<p>We should <strong>check that the migration generated has all the model definition</strong>. You can add to it extra indexes that eventually you had add previously.</p>
<p><figure class="tmblr-full" data-orig-height="281" data-orig-width="500" data-orig-src="./58391c80914eb86b0910c81d69fa1e1db96fa550b21853c3fbffed93f4665269.png"><img alt="image" src="./3cc12cd8beb0c2a2bf57917cd1943988d146554913001dc697c19a7d91fc0eeb.png" data-orig-height="281" data-orig-width="500" data-orig-src="./58391c80914eb86b0910c81d69fa1e1db96fa550b21853c3fbffed93f4665269.png"></figure></p>
<p></p>
<p></p>
<p><strong>3 - Just one entry in the __MigrationHistory</strong></p>
<p>Let's try to <strong>update the database running the Update-Database in PM, against the empty database</strong></p>
<p><figure class="tmblr-full" data-orig-height="367" data-orig-width="500" data-orig-src="./b20e1df8f7ce29ae6e56bf31a905d5a0837e52c633f248fa534da1e83593d8dc.png"><img alt="image" src="./c3a1e596d8dc4684405eeadfbf1978ec22c681b795479bfddd9186bb1d6d48c0.png" data-orig-height="367" data-orig-width="500" data-orig-src="./b20e1df8f7ce29ae6e56bf31a905d5a0837e52c633f248fa534da1e83593d8dc.png"></figure></p>
<p></p>
<p></p>
<p>Then <strong>check the __MigrationHistory table</strong></p>
<p><figure class="tmblr-full" data-orig-height="148" data-orig-width="500" data-orig-src="./b7289d0dc167d3771cc9289a764f217a4cfd8ddb24bef1a38eee2a7e12323e03.png"><img alt="image" src="./e896d3268320c46eadbcb9c25552d7937647047fdc1757f42c1ede81a682ae16.png" data-orig-height="148" data-orig-width="500" data-orig-src="./b7289d0dc167d3771cc9289a764f217a4cfd8ddb24bef1a38eee2a7e12323e03.png"></figure></p>
<p>It's ok. <strong>Just One entry in __MigrationHistory as we wish</strong>.</p>
<p><strong>4 - Do not compromise an upgrade to the current test environment</strong></p>
<p>Now, the interesting part. What happens if you try to <strong>update the database against the database that we are using so far, in which all migrations are registered</strong> in? Let's change Web.config to reuse again the correct connection string and try to upgrade database to see what happens</p>
<p><figure class="tmblr-full" data-orig-height="282" data-orig-width="500" data-orig-src="./8975664e83cf0aabb1501485a566d12292673a0a642150d92def2a7edb560c83.png"><img alt="image" src="./ef4e934da692d9a2d59854149795b9571516cd085b1027ee3d28b920f1d9ffeb.png" data-orig-height="282" data-orig-width="500" data-orig-src="./8975664e83cf0aabb1501485a566d12292673a0a642150d92def2a7edb560c83.png"></figure></p>
<p></p>
<p></p>
<p>Ok. It seems that there is no pending changes, which is true, since I didn't any change to my model. But I want to <strong>be sure that the changes from now on are applied correctly</strong>. So, let's do a minor change to check how the upgrade behaves.</p>
<p><figure class="tmblr-full" data-orig-height="222" data-orig-width="500" data-orig-src="./4800329601e1a535c8bd65dacbdec82b37b764c4bb34f17da8087139dcb86385.png"><img alt="image" src="./6771d16bfa1542b51b3fc7fb57d4a833d0700df08931212f20dd60c97317ef07.png" data-orig-height="222" data-orig-width="500" data-orig-src="./4800329601e1a535c8bd65dacbdec82b37b764c4bb34f17da8087139dcb86385.png"></figure></p>
<p></p>
<p>Basically we are adding a new column 'NewProperty' to an existing table. Let's create the migration</p>
<p></p>
<p><figure class="tmblr-full" data-orig-height="171" data-orig-width="500" data-orig-src="./0c3f01ad0930bd253ccc5a8d3029fd9838b408c2e2c0d84b9b7d8fb70ae814ce.png"><img alt="image" src="./af0fd15e667e1f842e7a379c40419f577015a6f0b7e03c76cdcc73cb65a6c586.png" data-orig-height="171" data-orig-width="500" data-orig-src="./0c3f01ad0930bd253ccc5a8d3029fd9838b408c2e2c0d84b9b7d8fb70ae814ce.png"></figure></p>
<p></p>
<p>And check the generated migration</p>
<p><figure class="tmblr-full" data-orig-height="173" data-orig-width="500" data-orig-src="./220eedf6297d2e05dd789391c970e66d21ae4639fb13a3573202f322cf27fb89.png"><img alt="image" src="./77052debfd887286b7d86e2f854dd6dc94c8416b9fdc4afaa3f5a4742081aa6b.png" data-orig-height="173" data-orig-width="500" data-orig-src="./220eedf6297d2e05dd789391c970e66d21ae4639fb13a3573202f322cf27fb89.png"></figure></p>
<p></p>
<p></p>
<p></p>
<p>So far so good. Now let's check what is the result to upgrade the database, running the Upgrade-Database command, but now I will use the<strong> -Script option to generate the script instead of running the upgrade</strong></p>
<p><figure class="tmblr-full" data-orig-height="150" data-orig-width="500" data-orig-src="./be10fbf97415326a6a65edb00429e94d1aef787ecb81b4fc6f84453f21b1f313.png"><img alt="image" src="./7e9e0b2fde9e04e7cf30b8c9f65da49ef78dd59aa99db974237edd0ba89c93c7.png" data-orig-height="150" data-orig-width="500" data-orig-src="./be10fbf97415326a6a65edb00429e94d1aef787ecb81b4fc6f84453f21b1f313.png"></figure></p>
<p></p>
<p></p>
<p>And here is the script</p>
<p><figure class="tmblr-full" data-orig-height="73" data-orig-width="500" data-orig-src="./80fd62e5e91c8928a9980a898c55cc9b8af48f182e1e0b968da859e72959ac9b.png"><img alt="image" src="./f5f17a51fd49555133952c249cd44f4c00f0a3825ea0ec87dd371301dd1fe5be.png" data-orig-height="73" data-orig-width="500" data-orig-src="./80fd62e5e91c8928a9980a898c55cc9b8af48f182e1e0b968da859e72959ac9b.png"></figure></p>
<p></p>
<p><strong><span>Ok! The change is applied and the migration is registered. Well done!</span></strong></p>
<p>Just one more note: f<strong>rom this point you can't downgrade to any migration that was excluded from the project,</strong> since the class is not in assembly anymore.</p>
<p><strong>Let's recap</strong></p>
<p>With this recipe you can reset migrations, allowing to have again an initial create migration which reflect the current model, which will be applied as a single migration in a clean installation, and at the same time, do not compromise upgrades on current deployed databases, where migrations are already registered.</p>