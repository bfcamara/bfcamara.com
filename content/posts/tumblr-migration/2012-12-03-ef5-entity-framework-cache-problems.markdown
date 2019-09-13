---
layout: post
template: post
date: 2012-12-03
tags:
- .Net
- EF
title: "EF5: Entity Framework cache problems"
slug: /post/37112709703/ef5-entity-framework-cache-problems
description: "EF5: Entity Framework cache problems"
---
<p>The majority of ORMs supports a first-level cache (L1), common named as "identity cache". Basically, if we request the same entity twice &nbsp;by its ID, it doesn't require a new round-trip to the database. EF5 is no exception, and provide us with the method <a href="http://msdn.microsoft.com/en-us/library/gg696418(v=vs.103).aspx" target="_blank">Find()</a> on the <a href="http://msdn.microsoft.com/en-us/library/gg696460(v=vs.103).aspx" target="_blank">DbSet&lt;T&gt;</a> class. From the documentation we have that Find</p>
<blockquote>
<p><em>Uses the primary key value to attempt to find an entity tracked by the context. If the entity is not in the context then a query will be executed and evaluated against the data in the data source, and null is returned if the entity is not found in the context or in the data source. Note that the Find also returns entities that have been added to the context but have not yet been saved to the database.</em></p>
</blockquote>
<p>Given that, if I make the call</p>
<div>

```cs
db.Contacts.Find(id);
```

</div>
<div><span><br></span></div>
<p>I know what is the expected behavior. The main problem exists with this code below</p>

```cs
//Get the contact for Id=1
var contact = db.Contacts.First(x => x.ContactId == 1)
Console.WriteLine("The Current Name is: " + contact.Name);

//Update the contact in the same session without using the context tracking
db.Database.ExecuteSqlCommand("update dbo.Contacts set Name = 'Filipe' where ContactId = 1");

//Get the contact again
var updatedContact = db.Contacts.First(x => x.ContactId == 1);
Console.WriteLine("The Updated Name is: " + updatedContact.Name);
```

<div><span><br></span></div>
<p>Assuming that the initial name is Andr&eacute;, what do you think is the output of this code? Here is the output</p>
<p><figure class="tmblr-full" data-orig-height="148" data-orig-width="500"><img src="https://66.media.tumblr.com/c6aa00b9aeebe99e9f98ea3f74a88fbd/2f6b4eb2bd18dd10-c7/s540x810/1744f857aaba6d0e20edbdf0e4cdf03280f01d08.png" data-orig-height="148" data-orig-width="500"></figure></p>
<p>To be honest, this is not what I expected to see. I was expecting to see Andr&eacute; followed by Filipe.</p>
<p>After looking at the code, I thought: well EF is smart enough to see that I am using the same query twice,&nbsp;</p>

```cs
db.Contacts.First(x => x.ContactId == 1)
```

<div><span><br></span></div>
<p>with the same params, in the same session, and is caching the data, then the 2nd call does not go to the database. Cool, I thought.</p>
<p>Let's confirm with the SQL profiler, expecting to see just one query</p>
<p><figure class="tmblr-full" data-orig-height="186" data-orig-width="500"><img src="https://66.media.tumblr.com/e5cde9cec95feb7ebd16d5c9dc16b998/2f6b4eb2bd18dd10-f6/s540x810/b6f4f499c02ee7d98d57d7c707e1ae39f238362f.png" data-orig-height="186" data-orig-width="500"></figure></p>
<p>I was totally wrong. The EF goes to database twice, getting the contact by Id. However, EF does not refresh the entity that is in context. Maybe this a a by design decision, to not sacrifice performance, since it would be required to merge the entity that is currently in context with the entity fetched from the database. However, in my opinion, it's hard to accept the fact that EF goes to the store to fetch the entity, and does nothing with it if the entity already exists in context.&nbsp;</p>
<div>
<div>To get the updated entity in context, we have two options :</div>
<div></div>
<div>1) detach the entity before querying it again</div>
<div></div>

```cs
((IObjectContextAdapter)db).ObjectContext.Detach(contact);
```

<div></div>
<div>2) Refresh the entity</div>
<div></div>

```cs
((IObjectContextAdapter)db).ObjectContext.Refresh(System.Data.Objects.RefreshMode.StoreWins, contact);
```

<div><span><br></span></div>
<div>If you opt for the Refresh option, it is not necessary to query again. Here is the final code</div>
<div></div>

```cs
//get the contact
var contact = db.Contacts.First(x => x.ContactId == 1)
Console.WriteLine("The Current Name is: " + contact.Name);

//update using ExecuteSqlCommand
db.Database.ExecuteSqlCommand("update dbo.Contacts set Name = 'Filipe' where ContactId = 1");

//Refresh the entity
((IObjectContextAdapter)db).ObjectContext.Refresh(System.Data.Objects.RefreshMode.StoreWins, contact);
Console.WriteLine("The Updated Name is: " + contact.Name);
```

<div><span><br></span></div>
<p>Here is the ouput</p>
<p><figure class="tmblr-full" data-orig-height="130" data-orig-width="500"><img src="https://66.media.tumblr.com/51dfe076c6fb2f35c092f4ad9cc6fe45/2f6b4eb2bd18dd10-f4/s540x810/d881ce3f6f2dd553a7d8d6fa6c8360f1bd2d7680.png" data-orig-height="130" data-orig-width="500"></figure></p>
<p>This a warning to all readers that can have this situation in a project, for example, when in the same context execute a stored procedure that modify entities that are already loaded in the current EF context, and for some reason you need to refresh the entities. Do not assume that by seeing the statement in the SQL profiler that EF will merge the data in context.</p>
</div>
<div></div>
<p><em><br></em></p>