---
title: Preparing Interviews for a Software Engineer position
date: "2020-12-21"
template: "post"
draft: false
slug: "/posts/preparing-interviews-for-software-engineer-position/"
category: "Job Interview"
socialImage: "preparing-interviews-for-software-engineer-position.jpg"
tags:
  - "Job Interview"
  - "Software Development"
  - "Software Engineering"
description: "I've just finished two job recuitment processes. Here are some
    tips and how I prepared them to succeed."
---
![Interview Preparation](./preparing-interviews-for-software-engineer-position.jpg)

Last week I've just **finished two recruitment processes for a Senior Software
Engineer position** that I have applied. Both companies have a very well organized
recruitment process, consisting in several interviews, including technical ones
with code challenges.

I got an offer in both cases. Unfortunately I had to decline one of them, and I
can tell you that it was a really hard decision, because both were presenting me
exciting and interesting challenges (engineeringly speaking). I wish I could
multiply myself to accept both offers (I would have loved to live both
experiences). But that's life. **And Yes, I am about to start a new journey. New
Year, New Life**.

In this post I want to **describe how I have prepared for the interviews and give
some tips** to someone thinking to apply to a similar position. **IMHO,
preparation is the key to succeed**.

## Be ready to write code in an interview

The obvious - if we are applying for a software engineer position, **we need to
be ready to be asked to write code during interviews**. I know that some
companies do not ask to write code in interviews, they assign you a homework
task instead. It's arguable which one is better.

In my case, in both processes I had to write code (C# and SQL). Furthermore, one
of the companies was  using a hiring platform,
[Codility](https://www.codility.com/), in which you need to write real working
code (compiled, tested against test data, etc.), not just pseudo-code on paper.

The thing is, if you write code on a daily basis, writing code is not an
issue, right? **The problem here is the type of problems that they are throwing
you in the exercises, particularly those that you don't deal with for some
time**. They tend to like exercises about:

+ Arrays challenges
+ Algorithms and data structures
+ SQL
+ Object oriented principles and design patterns

Let's see what kind of preparation we can think of to improve our ability to
solve these problems.

## Practicing Code/SQL challenges

There are some  **platforms out there where we can practice code/sql
challenges**. Here are some examples:

+ [Codility](https://codility.com)
+ [HackerRank](https://www.hackerrank.com/)
+ [LeetCode](https://leetcode.com/)

All of them have great exercises. Regarding difficulty level, **I would suggest
to focus on the Easy and Medium level**. You can choose the language of your
choice to solve the exercise.

Codility and HackerRank are hiring platforms, but they have a lot of training
content for developers. In Codility, the platform that I used most, I would
suggest the 10 first lessons in the section Codility for Programmers. HackerRank
is very thorough, and you can choose the skills you want to practice.

And if we struggle in a given exercise, we can have some help from the community
using those platforms. It happened sometimes, where I wasn't tackling the
problem in the right direction.

## Big-O, Algorithms and Data Structures

This is another topic that engineering interviewers like to discuss or see it in
action.

Today, when we need to sort some data, we typically rely on an implementation
provided by a base class library. So, if someone asks us out of the blue, what
is the difference between a bubble sort and merge sort, unless we have a great
memory or deal with this on a daily basis, probably we won't answer immediately.

But there are some theory that we should know for sure. One of them is the
[**Big O notation**](https://en.wikipedia.org/wiki/Big_O_notation) (asymptotic
analysis). And here, more important than to know what it is, it is **to
recognize quickly the approx. time complexity of a given algorithm**: constant,
logarithmic, linear, quadratic, etc.

So, even if we don't remember the bubble sort algorithm, as soon as we see its
implementation we should recognize a quadratic time complexity. Besides sorting,
searching algorithms is also important to be aware of, since they are somehow
related (we can adopt different searching approaches if the data is already
sorted).

A long time ago, I've read somewhere a saying: **_"Make it run, make it right,
make it fast"_**. I can't remember the source, but I can say that I really like
this approach. So, when the interviewer asks you to solve an exercise, **focus
first on the simplest solution**, and do not worry about performance. As soon as
you finish your first solution, try to identify its time complexity, and **if
you have time you can start to identify possible improvements** (unnecessary
work, common cases. etc.). In the coding platforms that I mentioned earlier, we
got a score based on the tests results. Sometimes the solution is
correct, but we don't get 100% score because we are not fast enough, and some
tests timeout.

For data structures, I think the important is to know **what data structure should
be used for a given scenario/exercise**. When facing an exercise we should try
to identify which data structure, or combination of data structures, best suits
the given problem:

+ Array
+ Linked List
+ Queue (FIFO - First in First Out)
+ Stack (LIFO - Last in First Out)
+ Hash table
+ Binary Search Tree
+ etc.

To know what is the best **data structure to use, we need to know the time
complexity of each of its operation**, such as adding/removing elements, search
by position, search by key, etc.

In my case, I reviewed all these concepts by taking a [course on
Pluralsight](https://www.pluralsight.com/courses/algorithms-data-structures-part-one).

## Object Oriented and Design Patterns

Another topic that interviewers like to ask is about **Object Oriented
Principles and Design Patterns**. And here we can expect several type of
interviewers: some of them asks you to explain the [SOLID
principles](https://en.wikipedia.org/wiki/SOLID); others that ask you to
write, for example a singleton pattern; and others that will present you a real
problem to see if you use principles and patterns in a natural way. I think this
later approach is the one that makes more sense, and in my case it happened
exactly that. I was asked to design a system where these topics started to popup
naturally. (interfaces, polymorphism, composite pattern, strategy pattern, etc.)

Again, if you use these principles on a daily basis, this should not be a
problem. Even some design patterns are so embedded in our daily work that we don't
even notice that we are using them. Nevertheless I decided to review these
topics, just in case someone could ask me about them in more detail.

Instead of reading the gang of four book, [Design Patterns: Elements of Reusable
Object-Oriented
Software](https://www.amazon.com/Design-Patterns-Object-Oriented-Addison-Wesley-Professional-ebook/dp/B000SEIBB8),
I decided to review just the most used patterns by reading the new edition of
[Head First Design
Patterns](https://www.amazon.com/Head-First-Design-Patterns-Object-Oriented/dp/149207800X),
which I really enjoyed. The Head First series books provides a great way of learning.

## Be ready to brainstorm about real problems

These are the questions that I like most, and in fact I was expecting more of
these. Basically **the interviewers present you a real problem**. Typically you
don't need to write any code. They are just ask you to present a high level
solution, discussing alternatives, pros and cons, etc. **They want to know how
you think**.

For example, _"the product owner wants a new feature of auto-completing a
search, providing multiple matches to the user, based on user past behavior.
Let's start thinking about the problem"_. That's the type of questions you
should expect.

**IMHO, this is the most interesting type of questions**.

## Be ready to be asked about past projects

Another common set of questions is **questions about past projects that are
mentioned in your CV**. My suggestion is to revisit your CV and try to remember
some options you have chosen, the design and architecture adopted, etc. If some
detail is missing in your head, go read some documentation that you still have
about the project.

Typically, **the projects that are discussed in interviews are chosen by you**,
not by the interviewer. So, be wise when choosing your projects to be discussed.

There are a lot of topics that can arise during these questions:

+ **DevOps general questions**: how you build? how you deploy? What flow you
  use? CI/CD?
+ **Testing approaches** (unit tests, integration tests, e2e test, load tests)
+ **Observability**: how do you know what's happening in production? Telemetry,
  traces, logs, etc.?
+ **Distributed systems**: pub/sub systems? queuing systems? retries? circuit
  breakers and other patterns, etc.

The variety of questions here is endless, and basically is based on your
experience.

## Know about the interviewer as much as possible

As I said earlier, **preparation is the key to success**. In an interview
process, part of the preparation is to know the interviewer. Most of the
companies announce who will be your interviewer. Take advantage of that
information and try to know as much as possible about that person via LinkedIn,
blog, presentations, etc.

If we think carefully, **interviewing is like dating**. Basically both parties
are trying to understand how compatible they are, right? If you know who is
interviewing you, you can pick common topics that you and your interviewer
relates to.

This _"investigation"_ can be very time consuming. Let me give you an example. For
the preparation of one of my interviews, I found a presentation made by the
interviewer. During that presentation he mentioned that he practices martial
arts. Great, I thought, I also practice martial arts (Taekwondo and Hapkido). So
we have something in common. At some point in the interview I asked
him what martial art did he practice, and we have spent a few minutes on this
topic. It's all about **Rapport - connecting through shared interests and empathy**.

For another interview, I've skimmed the master thesis of the interviewer about
Aspects Oriented Programming. I also read his blog. Again, some blog posts were
mentioned during the interview. We can create connections easier if we know
better who is talking to us.

## Soft skills

I am sure that there are a lot of other suggestions to excel in an interview. My
focus on this post was mainly in the Software Engineering field. I didn't want
to focus too much in the soft skills. I think the only suggestion that I have
regarding soft skills is: **be yourself and respect your identity**. Don't
pretend to be someone who you aren't.

## Conclusion

As a conclusion, I just want to mention that  a recruitment process is time
consuming for a candidate. **I spent many hours preparing my interviews**, and I
don't have any problem assuming that. Probably there are some people that don't
need any kind of preparation. Not me.

In this post I just wanted to describe how I prepared myself for the interviews.
It worked for me. It doesn't mean it will work for you.

Finally I want to finish this post by mentioning two resources:

1. the book [**Cracking the Coding
   Interview**](https://www.crackingthecodinginterview.com/). I didn't read the
   book so I can't recommend it. The reviews are great. I only heard about the
   book after my technical interviews. What I can say is that, if I knew it
   before, I would have read it before applying to those positions.

1. the site [**Glassdoor**](https://www.glassdoor.com/). Before applying to a
   position, search for interviews reviews on this site to have an idea what you
   should expect, how long does it take, how many interviews, etc.
