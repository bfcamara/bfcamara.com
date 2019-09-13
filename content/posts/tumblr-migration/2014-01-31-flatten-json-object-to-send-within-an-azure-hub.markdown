---
layout: post
template: post
date: 2014-01-31
tags:
- Azure
- Json
- c
title: "Flatten json object to send within an Azure Hub Notification Template"
slug: /post/75172803617/flatten-json-object-to-send-within-an-azure-hub
description: "Flatten json object to send within an Azure Hub Notification Template"
---
<p><strong>Have you ever needed to flatten your json to a bunch of properties? If yes, continue to read</strong>.</p>
<p>When we need to send a notification to an <a href="http://msdn.microsoft.com/en-us/library/windowsazure/jj927170.aspx" target="_blank">Azure Notification Hub </a>, we can choose from sending a native notification to Windows, Windows Phone, Apple or Google,&nbsp;or alternatively, <a href="http://msdn.microsoft.com/en-us/library/windowsazure/dn530748.aspx" target="_blank">use the agnostic method which uses templates</a>.</p>
<p><span>Currently, I am using the template approach, and here <a href="http://msdn.microsoft.com/en-us/library/windowsazure/dn369286.aspx" target="_blank">is the method signature</a></span></p>
<p><strong>public Task&lt;NotificationOutcome&gt; SendTemplateNotificationAsync(IDictionary&lt;string, string&gt; properties);</strong></p>
<p><span>So, basically we are only allowed to send key value properties.&nbsp;</span></p>
<p><span>Let's say that I have a string representing a json object, and I want to send it using this method.&nbsp;</span>To do this, <strong>I need to flatten my json (which can have nested objects and arrays inside it) and get a dictionary&nbsp;with all the keys.</strong></p>
<p>Here is an example of json object</p>
<p></p>

```json
{
	"name": "Bruno",
	"surName": "Camara",
	"address": {
		"street": "My Street",
		"number": 53
		"zipCode": "2840-344"
	},
	"familyMembers": [
		{ "name": "Filipa", relation: "wife" },
		{ "name": "Dekas", relation: "son" },
		{ "name": "Pipo", relation: "son" },		
	]	
}
```

<p></p>
<p>I want to transform this in a dictionary with the following set of key:value</p>
<p></p>
<pre>name:Bruno
surName:Camara
address.street:My Street
address.number:53
address.zipCode:2840-344
familyMembers[0].name: Filipa
familyMembers[0].relation: wife
familyMembers[1].name: Dekas
familyMembers[1].relation: son
familyMembers[2].name: Pipo
familyMembers[2].relation: son
</pre>
<p></p>
<p></p>
<p>Using the library <a href="http://james.newtonking.com/json" target="_blank">Json.NET</a> with Linq, it's something that we can do it relatively easy</p>
<p></p>

```csharp
private static Dictionary<string, string> GetNotificationPropertiesFromMessageContent(string messageContent)
{
	JObject jobject = JObject.Parse(messageContent);

	return jobject.Descendants()
		.Where(j => j.Children().Count() == 0)
		.Aggregate(
			new Dictionary<string, string>(),
			(props, jtoken) =>;
			{
				props.Add(jtoken.Path, jtoken.ToString());
				return props;
			});
}
```

<p></p>
<p>Basically we are querying for all nodes that are leafs, and in that case we add its path and value to the dictionary.</p>
<p>Here is a console application</p>
<p><figure class="tmblr-full" data-orig-height="280" data-orig-width="500" data-orig-src="./162290621c30bdbd9d23d7a9f9c69beabb0e58d872ff6eb06c502e227d8967da.png"><img alt="image" src="./583bff6b2b0ad15f7f20c9b33413cc64497af043922e395c8a497975f8007f55.png" data-orig-height="280" data-orig-width="500" data-orig-src="./162290621c30bdbd9d23d7a9f9c69beabb0e58d872ff6eb06c502e227d8967da.png"></figure></p>
<p></p>
<p>And here is the result of running it</p>
<p><figure class="tmblr-full" data-orig-height="292" data-orig-width="382" data-orig-src="./761ec9da70865833a26f9ec3e4a1541bd99852830e421162450567490dee2979.png"><img alt="image" src="./692442f7cf143edabf3993c26559c8c69b5b72bed9a234d7198a6ad3ad2a139f.png" data-orig-height="292" data-orig-width="382" data-orig-src="./761ec9da70865833a26f9ec3e4a1541bd99852830e421162450567490dee2979.png"></figure></p>
<p>I hope it helps.</p>
<p></p>
<p></p>