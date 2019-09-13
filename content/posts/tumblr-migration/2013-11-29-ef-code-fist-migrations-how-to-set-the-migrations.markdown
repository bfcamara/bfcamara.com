---
layout: post
template: post
date: 2013-11-29
tags:
- entity framework
- software development
- asp.net
- .net
title: "EF Code-Fist Migrations: How to set the Migrations folder"
slug: /post/68462303638/ef-code-fist-migrations-how-to-set-the-migrations
description: "EF Code-Fist Migrations: How to set the Migrations folder"
---
<p>In my <a href="http://www.bfcamara.com/post/68169174771/entity-framework-database-first-vs-code-first" target="_blank">previous post</a>, I confessed that I am currently using Code-First Migrations. If you are using Code-First migrations in your project, at some point in time <strong>you've had to enable Migrations, running on the Package Manager (PM) Console the Enable-Migrations command</strong></p>
<p><code> Enable-Migrations </code></p>
<p>Are you curious what this command does? Basically, it looks for the current project to find a DbContext, and <strong>generates a new class with the name Configuration, which is placed under the folder with the name Migrations in your root</strong>, with the following code</p>

```cs
namespace Myapp
{
    using System;
    using System.Data.Entity;
    using System.Data.Entity.Migrations;
    using System.Linq;

    internal sealed class Configuration : DbMigrationsConfiguration
    {
        public Configuration()
        {
            AutomaticMigrationsEnabled = false;
        }

        protected override void Seed(MyApp.Data.MyDbContext context)
        {
            //  This method will be called after migrating to the latest version.

            //  You can use the DbSet.AddOrUpdate() helper extension method 
            //  to avoid creating duplicate seed data. E.g.
            //
            //    context.People.AddOrUpdate(
            //      p => p.FullName,
            //      new Person { FullName = "Andrew Peters" },
            //      new Person { FullName = "Brice Lambson" },
            //      new Person { FullName = "Rowan Miller" }
            //    );
            //
        }
    }
}
```

<p>Every time you want to add a new migration to your project, you have to run in PM console the Add-Migration command</p>
<p><code> Add-Migration AddColumnFirstName </code></p>
<p>Here, AddColumnFistName it's the Migration name, and is just an example of a migration.</p>
<p>After this command, you will have a new class 201311291316579_AddColumnFistName added to you project,<strong> which is placed under the folder with the name Migrations</strong>. The name of the class is generated following the pattern &lt;current_date_time&gt;_&lt;migration_name&gt;.</p>
<p>Below it's an example of the code generated for the migration</p>

```cs
namespace MyApp.Migrations
{
    using System;
    using System.Data.Entity.Migrations;
    
    public partial class AddColumnFirstName : DbMigration
    {
        public override void Up()
        {
            AddColumn("dbo.Person", "FirstName", c =&gt; c.String());
        }
        
        public override void Down()
        {
            DropColumn("dbo.Person", "FirstName");
        }
    }
}
```

<p><strong>I don't like the location of the Migrations folder, which is being placed on the root of my project. Fortunately, you can set this location</strong>. For example, I want to place my Migrations in the Data folder. To do this, when enabling migrations, I have to pass the MigrationsDirectory parameter to the Enable-Migrations command</p>
<p><code> Enable-Migrations -MigrationsDirectory Data\Migrations </code></p>
<p>Besides of the new location of the Configuration class, there is one more difference in the generated code, in the Configuration constructor</p>

```csharp
public Configuration()
{
    AutomaticMigrationsEnabled = false;
    MigrationsDirectory = @"Data\Migrations";
}
```

<p><strong>From now on, every time I add a new migration, this will be added to the Data\Migrations folder. I like to have a clean root project folder</strong>.</p>
<p>&nbsp;</p>
<p>Just one note: <strong>if you already have the Migrations folder in your root, and you wish to have a new location, I suggest to do it manually</strong>, moving all the content of the current migrations folder to the new location. Do not foget to change the Configuration constructor as well, setting the MigrationsDirectory property with the new location.</p>