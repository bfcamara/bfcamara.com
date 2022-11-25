---
title: Chasing a failed test in CI/CD pipeline
date: "2021-03-14"
template: "post"
draft: false
slug: "/posts/chasing-failed-test/"
category: "Software Development"
socialImage: pipeline_man_walking.jpg
tags:
  - "Unit Testing"
  - Troubleshooting
  - Release
  - CICD
description: "This post describes the troubleshooting experience of having a test failing in
CI/CD pipeline, which was not easy to reproduce locally in my machine."
---
![You have broken the CI/CD pipeline](./pipeline_man_walking.jpg)

## 😨 You have broken the CI/CD pipeline

A couple of weeks ago I spent some time investigating a test that started to
fail in CI/CD pipeline after I have merged a pull request. **When a test starts
failing in CI/CD, the faces of the people with commits that triggered that CI/CD
run go to a public dashboard, visible by the whole R&D team**. My face was there
🤦. Not a great feeling. I've assigned the failing test to myself. I've
put the label  `I'm on it` on the test.

## 💻 It works on my machine

The test was not new, and it was doing some odd stuff in the arrange part of the
test in order to setup some behavior in the database related with nested
transactions. My pull request was making changes in the database, including a
new version of a stored procedure that is being called in that test. I thought
it could be something on the stored procedure itself. **The curious thing is
that I couldn't reproduce the error in my machine when I was running the test
inside Visual Studio, or even using the same runner app that CI/CD pipeline was
using.**

## 🕵️‍♂️ Investigation

After spending some time troubleshooting I got the conclusion that the problem
was not in the database changes. **So what could it be?** With the help of my
team, I understood that **in CI/CD pipeline the tests run against a release
build. Not only release, but also a build with an obfuscation process in
place**. So the next step was to run the test in the exact some conditions. And
guess what - the test started to fail in my machine. It should be the
obfuscation process, I thought. Let's remove obfuscation out of the equation -
the test was still failing with just a release build without obfuscation.

Next step was to try to **understand the differences between the Build and
Release configuration**. We also checked what were the differences at the [.Net
Intermediate Language
(IL)](https://docs.microsoft.com/en-us/dotnet/standard/managed-code#intermediate-language--execution).
Nothing relevant from these differences that could explain the behavior of the
test.

One member of my team then have tried to apply the `NoOptimization` attribute to the test
method. Then the test started to pass again 💥.

```csharp
[Fact]
[MethodImpl(MethodImplOptions.NoOptimization)]
public void Test_method_here() {
  // ... test code goes here
}
```

From the
[documentation](https://docs.microsoft.com/en-us/dotnet/api/system.runtime.compilerservices.methodimploptions?view=netframework-4.7.2)
we can see what the `NoOptimization` means

> The method is not optimized by the just-in-time (JIT) compiler or by native
> code generation (see Ngen.exe) when debugging possible code generation
> problems.

With this change, my face was not anymore on the public dashboard. But I was not
happy with the fix. This code was just a test, but it's not hard to imagine that
some production code could have the same issue. **The `NoOptimization` could be
only masquerading the real issue**. It seems that the JIT compiler is doing
something different for the release build. I plan in my next post to explain what
the test was doing, why it was failing in release, and how I have fixed it
without the need to rely on compiler services options.
