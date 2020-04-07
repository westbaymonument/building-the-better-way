---
layout: default
title: Framework Selection
parent: Testing
grand_parent: Development Guidelines
nav_order: 4
---

# Test Framework Selection
{: .no_toc}
You can apply the principles in this document when using any test framework. At their core, .NET test frameworks simply provide a runner to find and call your test methods, treating exceptions as failures. However, when Headspring selects a .NET test framework for a project, we can guide our clients towards the most effective choice, based on our experiences across [MSTest](https://docs.microsoft.com/en-us/dotnet/core/testing/unit-testing-with-mstest), [NUnit](https://nunit.org/), [xUnit](https://xunit.net/), and [Fixie](https://fixie.github.io/).

1. TOC
{: toc}

### **DO** favor xUnit
{: .text-green-100 }
xUnit built upon many of the design choices in NUnit. Test classes are constructed once for each test case, making it easier to avoid shared state and easier to perform test setup via plain old constructors. Due to the simpler model, there is less pressure to create complex inheritance in test classes.

The xUnit team was by far the most aggressive in tackling the complications of .NET Core, so it has the best overall support across all versions of .NET and Visual Studio. Because the ASP.NET and .NET Core teams have embraced xUnit, it is also the most likely to be maintained in the long run.

Consider the following downsides when selecting xUnit:

* When setting up xUnit for the first time, we recommend disabling its automatic parallelism unless you have a specific need to take advantage of it. It is difficult to avoid having parallel tests interfere with each other in misleading ways, especially for integration tests.
* Regular console output is suppressed by the test runner. You may find yourself unable to Console.WriteLine within a problematic test or Subject Under Test (SUT) as a debugging technique.

### **CONSIDER** Fixie
{: .text-yellow-300 }
[Fixie](https://fixie.github.io/) built upon many of the design choices in xUnit, so it is best contrasted directly with xUnit. With Fixie:

* The assembly loading model is far simpler. Your test project is simply a console application running from the test project’s `bin/…` folder. At the time of this writing, similar is planned for xUnit 3.
* The default convention requires less boilerplate code. A test class is just a plain old fashioned C# class with familiar semantics.
* Test execution is naturally single-threaded.
* Customizing the test class lifecycle is far easier.
* Customizing the meaning of test method parameters is far easier.

Naturally, our clients have little or no exposure to Fixie, so we’re more likely to select this for projects predominantly written/maintained by Headspring.

### **AVOID** NUnit
{: .text-red-300 }
NUnit has a complex test class lifecycle, which in practice motivated several test design smells:

* Shared state across tests, intentional or not, due to the fact that a test class instance is reused across its many tests.
* Complex test setup steps using a mix of test class-level setup and test method-level setup.
* Difficult-to-trace inheritance to share common test class-level and method-level setup for cross-cutting concerns.

NUnit has also been slow to adapt to the .NET Core era of tools, meaning that the NUnit team is encountering and addressing complications that xUnit and Fixie had already solved years earlier. We should avoid selecting NUnit for new projects.

### **AVOID** MSTest
{: .text-red-300 }
MSTest suffers from the same complex test class lifecycle as NUnit. It has also been given little attention from Microsoft in recent years. Even the ASP.NET and .NET Core teams have abandoned using it in favor of xUnit. We consider MSTest to be a legacy tool that we will only bother using when a client’s project already has an established test suite.
