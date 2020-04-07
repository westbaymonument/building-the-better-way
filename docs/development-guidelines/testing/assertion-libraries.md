---
layout: default
title: Assertion Libraries
parent: Testing
grand_parent: Development Guidelines
nav_order: 5
---

# Assertion Libraries
{: .no_toc}

1. TOC
{: toc}

### **DO** use extension methods for assertions
{: .text-green-100 }
Most test frameworks come with their own built-in assertion classes. They are often based on NUnit’s model, where assertions work as static method calls:

```csharp
Assert.AreEqual(expected, actual);
```

These were appropriate at the time NUnit was first released, but later C# versions gave us a much more useful alternative. Shouldly allows us to use extension methods:

```csharp
actual.ShouldBe(expected);
```

These extension methods are more natural to write and to read, and we can augment the built-in assertions with application-specific shorthand, simply by defining our own extension methods with the “ShouldXyz” style of method naming.

### **DO** favor the Shouldly assertion library over a test framework’s built-in static assertions
{: .text-green-100 }
Among .NET assertion libraries, [Shouldly](https://github.com/shouldly/shouldly) has a simple API, is actively maintained, and works well for .NET Framework and .NET Core.
