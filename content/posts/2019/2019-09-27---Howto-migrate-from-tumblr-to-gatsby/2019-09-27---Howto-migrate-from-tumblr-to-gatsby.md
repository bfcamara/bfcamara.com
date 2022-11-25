---
title: From Tumblr to Gatsby
date: "2019-09-27"
template: "post"
draft: false
slug: "/posts/howto-migrate-from-tumblr-to-gatsby"
category: "Development"
tags:
  - "gatsby"
  - "jamstack"
  - "netlify"
description: "Howto migrate from Tumblr to Gatsby "
---

![Gatsby](./Gatsby_Logo.png)

I've recently migrated this blog from [Tumblr](https://tumblr.com) to
[Gatsby](https://www.gatsbyjs.org).

I've used Tumblr for many years to host this blog. Tumblr can be used for that
purpose, but it also provides some other social features like _follow_, _like_,
_reblog_, links and video sharing, etc. Moving my blog to a static site
generator was in my backlog for a long time, and the main reason is to not have
my content dependent of any 3rd party service like Tumblr, Medium,
Wordpress, etc. Nobody knows what these services will do in the future with our
content. Look for example at Medium, it's very annoying every time I need to
read an article on Medium to be asked to upgrade to a paid plan to continue
reading, and forces me to navigate incognito to workaround that. Anyway...

There are many [static site generators](https://www.staticgen.com), and some of
them that are very popular ([Jekyll](https://jekyllrb.com/),
[Hugo](https://gohugo.io/), etc.). So, the question is, _"which one should you
use?"_. Well, there is no right or wrong answer. I've chosen
[Gatsby](https://www.gatsbyjs.org), and here are the reasons:

+ it uses [React](https://reactjs.org/) as templates
+ it uses [GraphQL](https://graphql.org/) to query data sources to generate the site
+ it is written in JavaScript
+ it has gained a lot of attention recently
+ it's easy to learn if you know node and React
+ it has a [lot of plugins](https://www.gatsbyjs.org/plugins/) to integrate with
+ it has a [lot of starters](https://www.gatsbyjs.org/starters/?v=2) that you can
   use as a starting point for your blog, portfolio, company, or e-commerce site.

In summary, I've chosen Gatsby because I want to deep my knowledge in the
components that uses (React, GraphQL, JavaScript, etc.), and due to its recent
popularity in the community.

## Blog Migration Requirements

Here is the requirements list for my blog migration:

+ The content of old posts should be migrated to Markdown files (to be hosted
   locally in my git repo)
+ The design should be very simple and clean, allowing me to have an "About" page
+ It must be able to integrate with [Disqus](https://disqus.com/), to allow
   readers to comment my posts.
+ Old URLs must be kept (no broken links), including the RSS URL to keep
   the subscription of my blog in feed readers.
+ All the typical requirements of a blog about software development: tags,
   categories, code snippets displayed friendly, google analytics, etc.

## Migration of Tumblr content to Markdown

The first step was to get the content of my blog posts as markdown files,
including images used in posts. [Tumblr provides a REST
API](https://www.tumblr.com/docs/en/api/v2) that I can use to grab my content. I
used a python script,
[tumblr2markdown.py](https://github.com/jaanus/tumblr2markdown/blob/master/tumblr2markdown.py)
from someone that had the same need. It worked great, generating a markdown file
for each post, and downloading all the images embedded in posts. Each markdown
generated contains a YAML front matter with metadata that will be processed by
Gatsby. I just needed to do some small changes in the python script to include
more metadata, in particular the `slug` field to be set as the same URL used in
Tumblr. (more on that later)

Here is an example with my first post, written in 2012.

```md
---
layout: post
template: post
date: 2012-01-28
title: "Welcome to my blog"
slug: /post/16629534467/welcome
description: "Welcome to my blog"
---
<div>&#13;
<p>One of my resolutions for 2012 was back to blog . I decided to kill <a href="http://www.agilior.pt/blogs/bruno.camara/" target="_blank">my old blog</a> (in fact, it was already dead),  which is hosted in the <a href="http://www.agilior.pt/blogs/" target="_blank">Agilior's corporate blog</a>, the company where I work and I was a co-founder (right now, I already  haven't  any part of the company's capital) and create a new blog with a personal touch,associated with the bfcamara.com domain.</p>&#13;
<p>The most of the content that I will publish is certainly related to my work, software development.</p>&#13;
<p>As one of the founders of Agilior, I hope also to share some of the stories that were part of my life as founder, and some of the mistakes I made along this route. There will be space also to personal subjects, however with lower frequency.</p>&#13;
<p>In <a href="http://www.agilior.pt/blogs/bruno.camara/archive/2007/12/02/3300.aspx">one of my posts</a> at the old blog, I explain that my life has been done of cycles. My perception is that a new cycle is about to come. I do not expect an easy time, and certainly I will have to leave the comfort zone. But "freedom" is one of the things that I value most, and to be honest,  I like to be the leader and the captain of the ship", deciding where to go and when.</p>&#13;
<p><span>I hope I can maintain some activity in this blog.</span></p>&#13;
</div> 
```

As you can see, the content hosted in Tumblr is not very clean, probably because
I was using a rich text editor in Tumblr to write my posts, but in the end, what
is important is that posts are being displayed correctly in the new blog.

## Choose the design by using a Gatsby starter

The next step was to choose the design for my new blog. I wanted a simple and
clean design. I started to look to the [Gatsby starters
library](https://www.gatsbyjs.org/starters/?v=2), and I've tried some of them.

I ended by choosing the [gatsby-v2-start-lumen
starter](https://www.gatsbyjs.org/starters/GatsbyCentral/gatsby-v2-starter-lumen/)
starter. The design is simple, clean, and it's already prepared to integrate
with Disqus. However, in terms of code, I did some changes and I removed some
features that I didn't want to use:

+ Removed integration with [Flow](https://flow.org), a static type checker for
  JavaScript. I thought of using [TypeScript](https://www.typescriptlang.org/)
  instead (also brings optional static type-checking to JavaScript), but in the
  end I've changed the code to be just plain JavaScript (ES2018).
+ Removed docker files
+ Changed the [Prismjs](https://prismjs.com/) theme to Okaidia. Prismjs is a
  syntax highlihter that this Gatsby starter uses to make code snippets
  embedded in my posts to look great.

Finally, I just needed to set some settings: Avatar, Google Analytics
key, Twitter handler, etc, and I had my blog up and running.

## Choose the platform to host the blog

Next I needed to choose which platform to use to serve my blog. I had simple
requirements: free, support custom domains, and support automatic deployments.
By automatic deployments I mean having a new deployment triggered whenever I do
a `git push` in my master branch.

There are many platforms that support [JAMstack](https://jamstack.org/) sites.
A JAMstack site (JAM = JavaScript, APIs and Markup) doesn't need a web/app
server. Since I am using Gatsby, which is a static site generator, my blog follows the
JAMstack. Here is a list with some platforms that I could have used:

+ [Netlify](https://www.netlify.com/)
+ [GitHub pages](https://pages.github.com/)
+ [Surge](https://surge.sh/)
+ [Zeit](https://zeit.co/)

There are many others. I started with Netlify due its popularity, and I must say
that the experience was awesome. So, I decided to go with Netlify. But from what
I've seen from documentation of other platforms, I would risk to say that I
wouldn't have any issue if I had chosen one of them.

In the end, I've created my site in Netlify, configured with my custom
domain (bfcamara.com), and configured  with automatic deployments from [my git
repo](https://github.com/bfcamara/bfcamara.com) hosted in GitHub.

### Keep old URLs (no broken links)

One of my main requirements was to keep the old URLs. Basically I didn't want
to loose all the links out there pointing to my old blog, including the RSS link
that people were using to subscribe to my blog.

Part of the problem was solved during content migration. Each markdown created
during migration was generated with a front matter key, the `slug` key, with its value
set to the URL used in Tumblr. Gatsby uses this key to determine the URL of a
post. For example, for my welcome post I have

```yaml
slug: /post/16629534467/welcome
```

The issue is that Tumblr has configured automatically some redirect rules:
`/post/16629534467` is configured to be redirected to `/post/16629534467/welcome`.
And Tumblr does this for all posts, so the post ID is enough for a post to be visited.
I needed to the same in my new blog by using redirects. 

[Netlify supports redirects](https://www.netlify.com/docs/redirects/), and I just
needed to add a new file `_redirects` to my repo with some rules. For
the welcome example above, it's just as simple as

```
/post/16629534467 /post/16629534467/welcome
```

You can check
[here](https://github.com/bfcamara/bfcamara.com/blob/master/static/_redirects)
the file with all the rules.

Finally, I also wanted to keep the RSS subscription URL. In the old
blog I was using `/rss` and the new blog is using `/rss.xml`. I just needed to
add a final redirection rule

```
/rss /rss.xml
```

With this rule, RSS readers doesn't need to change my blog subscription URL.

### Conclusion

In conclusion, I am happy with the final result, mainly because now I have full
control and I own the content of my blog. I also took advantage of this process
to learn Gatsby. I hope it helps anyone that needs to migrate a blog to a static
site generator. You can check the [git repo in GitHub](https://github.com/bfcamara/bfcamara.com).