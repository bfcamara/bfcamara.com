---
title: Visual Studio 2022 - Remote Testing
date: "2022-12-21"
template: "post"
draft: false
slug: "/posts/vs2022-remote-testing/"
category: "Software Development"
socialImage: /media/vs2022-remote-testing.png
tags:
  - "VS 2022"
  - "Unit Testing"
  - "Containers"
  - "WSL"
description: |
  This post shows how Remote Testing works in Visual Studio 2022, by running and 
  debugging a unit test in a WSL Ubuntu distribution.
---

![Visual Studio 2022 - Remote Testing](vs2022-remote-testing.png)

Several months ago I wrote a [blog post explaining how could we debug a unit
test running in a Linux container using Visual Studio
2019](/posts/vs2019-debugging-unit-test-in-container/). In that post I mentioned that
the experience is far from being great in VS2019. It was possible, but it
required some manual steps.

When we want to debug a unit test running in a Linux container, our
**expectation is to have the same experience as when debugging locally**:

    1. Go to Test Explorer
    1. Right Click on the Test and select Debug
    1. The debugger starts and stops in a breakpoint inside the test
  
That's exactly what **Visual Studio 2022 brought to us with [Remote
Testing](https://docs.microsoft.com/en-us/visualstudio/test/remote-testing?view=vs-2022)**. Let's see how it works.

Create a [xUnit](https://xunit.net/) test project.

![xUnit test project](vs2022-create-xunit-test-project.png)

Choose .NET 6.0

![xUnit test project .NET 6.0](vs2022-create-xunit-proj-net6.png)

For demonstrating purposes, let's create the same test used when demonstrating how to debug in Visual Studio 2019. This is a **simple test that asserts that the test is running in a Linux container**.

```cs
  [Fact]
  public void Test_is_running_in_linux_container() {

      Assert.True(RuntimeInformation.IsOSPlatform(OSPlatform.Linux);

  }
```

Now add a new file `testEnvironments.json` to the root of the solution

```json
{
  "version": "1", // value must be 1
  "environments": [
    {
      "name": "WSL-Ubuntu",
      "type": "wsl",
      "wslDistribution": "Ubuntu-20.04"
    }
  ]
}
```

Basically I am **adding a new test environment to run my tests in a WSL Ubuntu
20.04 distribution**. To check the list of distributions you can run `wsl
--list` in a command prompt. In Test Explorer you now have a dropdown with the
list of test environments. Let's select the test environment just added,
`WSL-Ubuntu`.

![VS2022 - Select Test Environment](./vs2022-select-test-environment.png)

**Note**: if you don't see the new environment, then close Visual Studio and launch it
again. This is an experimental feature and I have experienced some issues while
playing with this feature.

Put a breakpoint in the test that you want to debug and select it in Test
Explorer to start debugging it.

![VS2022 - Start Debug Test](vs2022-start-debug-test.png)

Debugger stops at the breakpoint

![VS2022 - Debugging](vs2022-debugging-unit-test.png)

And the test passes.

![VS2022 - Test Passing](vs2022-test-passing.png)

Change the environment to `< Local Windows Environment >` and you will see that
the test fails.

![VS2022 - Test Failing](vs2022-test-fails.png)

Thank you Visual Studio 2022 for greatly simplify this experience of remote testing.
