---
title: DDD is fancy - Are we doing it right? Should we use it?
date: "2020-10-23"
template: "post"
draft: false
slug: "/posts/ddd-is-fancy-are-we-doing-it-right/"
category: "Software Development"
socialImage: /media/DDD_Praise_from_Eric_Evans_Book.jpg
tags:
  - "DDD"
  - "Software Development"
description: "This post is a reflection about Domain Driven Design (DDD)."
---

![Domain Driven Design](./DDD_Praise_from_Eric_Evans_Book.jpg)

**TL;DR** - This post is just a reflection about DDD based on my experience,
and my goal is to reflect about some questions: **are we doing DDD right? is it
worth?** I am far from being an expert in Domain Driven Design (DDD). So, you
should skip the entire post if you expect some words from a DDD expert
practitioner.

## How are we doing DDD

If someone says "My team uses DDD", that person gets my full attention.
Basically I enter in a mode where I can do only two things: to breathe and to get
more info from the person in front of me to know more about how they are doing
DDD.

Basically, I will get a lot of curiosity to know more about the project, and
particularly to know **how complex the project is, and what is the layered
architecture being adopted**. In my opinion, I think it's dam hard to do DDD
right (whatever that means). Furthermore, I will always ask if I can take a look
at the code to see **what DDD concepts are being used** (entities, value
objects, aggregate roots, repositories, unit of work, etc.), and **what software
architecture/patterns are in place** (Hexagonal Architecture, Onion
Architecture, CQRS, Event Sourcing, etc.). My hope is always to get something
new that I can learn from it, or something new that I can say _"this makes a lot
of sense and I've never though about it. I need to try for myself"_. **Keep
learning is always the goal**.

## Keep studying DDD

I think it was around 2005 when I read the Eric Evan's **book ["Domain Driven Design: Tackling
Complexity in the Heart of
Software"](https://www.amazon.com/Domain-Driven-Design-Tackling-Complexity-Software/dp/0321125215),
and I can say that it was very hard to digest it**. By that time, my
feeling was that there were missing good samples to serve as base reference
implementation. It started to appear then some samples in the community, but I never
got 100% happy with a single one, where I could say _"That is the one"_. Even
today, from time to time, I **still search for new DDD samples to see how they
are tackling software complexity**.

Fortunately, we have more samples today that we can learn from, at least the
tactical aspects of a DDD implementation. I say tactical, because regarding the
domain knowledge we don't have access to the decisions that were made that have
conducted to the given model. And that's where **I think we (as developers) are
missing one of the most important points of Domain Driven Design: the Ubiquitous
Language**.

## The Ubiquitous Language

I don't know why, but **we (as developers), tend to think that we know more of a
given domain than the domain expert itself**. It doesn't make any sense - but we
still do it a lot. That's why there is a **gap in the communication between the domain
experts and the technical team members**. We tend to build something that
follows our mental model based on our own technical language, and not based on the domain
expert language. **We should build based on the Ubiquitous Language**.

Unless we are building software that targets developers itself, **we are not the
domain experts**. So we should listen them first, ask them questions to make
them think deeper about the subject, and then **agree with them on a common
vocabulary and model** that is understood by all team members in the project.
That's the model that we should build in software. That's why I think that **it
makes a lot of sense to have a domain expert as a member of the team. If not,
are we really doing DDD? And if we don't need a domain expert on the team, are
we really tackling a complex domain that justifies a DDD investment?** Probably
not.

## Technical complexity

If we are not tackling a complex domain, I think we should focus in not adding
technical complexity - **instead we should chase for simplicity, technically
speaking**.

For example, we are asked to **build a simple CRUD application** - a small
backofice web app to manage some reference data. Why should we add technical
complexity to this simple app? **Probably we can get most of the job done by using
a scaffolding tool**, combined with a validation, authorization, and auditing
mechanism and we are good to go, right? **Is there anything wrong with this
approach?** I don't think so.

Now, imagine that we are developing **a core domain backend service to be
consumed by a frontend** (a [BFF](https://samnewman.io/patterns/architectural/bff/)-Backend for
Frontend). This **core domain
is the one that generates money for the company and it's relatively complex, and
it's evolving quickly**.

We **decide to use [GraphQL](https://graphql.org/) to expose our API** to be
consumed by the frontend, which is mostly a **Task based UI**. Since we are
using GraphQL, we can think that it makes sense to use internally different
approaches for queries and mutations, and so **we decide to implement a
[CQRS](https://www.martinfowler.com/bliki/CQRS.html) pattern** (Command Query
Responsibility Segregation). **For the Commands, the rules are complex, and so
we decide to adopt a Domain Model approach**, with persistence ignorance,
repositories, triggering domain events (potentially to be consumed by other
services), etc. But for the **Queries we decide to use a simpler approach by
using a Micro-ORM with an Anemic Read Model** (or DTOs if you prefer).
Initially, we can even decide that both Queries and Commands are using the same
physical database, but we can change that in the future if we need it, by
physically isolating the read and write persistence model. **Is there anything
wrong by mixing these approaches?** I don't think so. Are we really using DDD?
Probably **we are only using some DDD tactical patterns in parts of our
system**.

## Don't need to be "DDD All-In"

**I think DDD is not an All or Nothing approach**. We can use a **DDD-Lite** (term
coined by Vaughn Vernon in his book [Implementing Domain-Driven
Design](https://www.amazon.com/Implementing-Domain-Driven-Design-Vaughn-Vernon/dp/0321834577))
by **choosing just some DDD tactical patterns for parts of our system**.

**I think also that DDD is not for all projects**. We all like to put in
practice what we learn, and we all want to become better developers writing
better software today than we wrote yesterday. **DDD is something that I think
every developer should learn in the course of his career. But more important is
to get the capability to decide when it's worth or not to use DDD in a
project**. If we decide to adopt DDD blindly, regardless of the complexity of
the domain, just because we are proud of our brilliant code with lots of
patterns, I think it's a wrong approach. **At the end of the day we are hired to build
software that is supposed to add value to the business**. And with this blind
approach, we are just throwing technical complexity to the solution, which
becomes a cost.

## Conclusion

To conclude, I think **we should keep learning DDD, to be able to apply it when it
worths, and no less important also, to be able to know when it doesn't worth**.
