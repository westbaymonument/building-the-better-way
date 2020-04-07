---
layout: default
title: Coverage
parent: Testing
grand_parent: Development Guidelines
nav_order: 2
---

# Test Coverage
{: .no_toc}
We expect the suite of tests produced for a project to adequately address all core areas of system functionality.  Furthermore, each test fixture should contain a set of tests that fully exercises the features of the code being tested.  We are not targeting a specific code coverage percentage as that can easily become a vanity metric or be gamed with low-value tests containing trivial assertions.

Each project is different, and may have different testing requirements (for instance, some projects need UI testing or Integration tests with specific external services). In any case, the following are core areas that should _always_ have coverage:

1. TOC
{: toc}

### **DO** exercise all of the functionality of the system under test
{: .text-green-100 }
Test coverage is important.  We want to be sure we test all areas of the code we’ve written.  A good rule of thumb is to expect at least one test for each public method exposed by the system under test, multiplied by the number of critical logical input states to that method.

Use some judgement here.  If your method takes a huge range of inputs, you don’t need to write a separate test case for every possible input.  You should, however, be able to collapse your inputs down to groupings that produce meaningful coverage. Coverage is about confidence levels: are you confident that the set of examples meaningfully demonstrates how the code behaves?

### **DO NOT** chase code coverage metrics
{: .text-red-300 }
When you have a code coverage tool, it’s tempting to write or change tests merely to improve the reported coverage metrics. It is far too easy to “game” these metrics with tests that happen to touch more lines of code without exercising meaningful scenarios and without making meaningful assertions. When the metric drives the testing, the tests tend to suffer while giving the team false confidence.

### **AVOID** code coverage tools as the primary measure of quality
{: .text-red-300 }
Our default recommendation is code coverage tools can provide a false sense of security because it provides a concrete metric front and center. We find using thorough [code reviews](/development-guidelines/pull-requests/) of all feature branches achieves the same end goal of ensuring there is quality test coverage, and that a human can make a much better evaluation of the coverage compared to static analysis metrics. Because of this, we find that using them can cause more harm that good especially if the intention is not internalized by the entire development team.

Code coverage metrics can be valuable as a _secondary_ line of defense. We recommend JetBrains’ [dotCover](https://www.jetbrains.com/dotcover/). When evaluating an alternate a test coverage tool, ensure it supports .NET Core and systems like [Azure DevOps](https://azure.microsoft.com/) - as some test coverage tools have not beeen maintained for latest advances.

### **DO** test Queries / Commands for reading/writing data
{: .text-green-100 }
[Query and Command handlers](https://github.com/jbogard/MediatR/wiki#basics) form the vast majority of the system under test. Often these tests assert on the reads and writes performed by the handler. See the section on [Integration Testing Patterns](/development-guidelines/testing/integration-test-patterns.html#integration-test-patterns) for more on effectively testing these handlers.

### **DO** test core infrastructure / helper classes and extension methods
{: .text-green-100 }
Helper methods that are used as building blocks to implement business logic often deserve direct testing, which then gives you the confidence to use them in the rest of the system.

### **DO** write test setups based on real world scenarios
{: .text-green-100 }
Realistic scenarios give us confidence about how the system will behave in production. They help to ensure that our high coverage comes not from merely hitting every line in the system under test, but doing so in ways that reflect production.

### **DO** address all expected inputs to the system under test
{: .text-green-100 }
This includes external system state such as environment variables, file system content, and database content.

### **DO** probe edge cases
{: .text-green-100 }
Edge cases are states we may not expect to see frequently or where the code should always behave differently. These are frequent sources of bugs. Consider carefully the inputs to a method you know require special handling and write tests specifically for those cases.  For example, any input value less than or equal to 1 would be a special case in this code, and would warrant its own test case:

```csharp
int Factorial(int n)
{
    if (n < 0)
        throw new InvalidOperationException();

    if (n == 0 || n == 1)
        return 1;

    return n * Factorial(n-1);
}
```

### **DO** verify behavior under exceptional conditions
{: .text-green-100 }
Confirm the behavior of the system when given invalid inputs. Confirm the behavior of the system when the code is expected to throw an exception.

### **DO** verify the full range of outputs and side effects
{: .text-green-100 }
Full validation of the behavior of a class can be tricky to get right.  Remember when writing your test that input isn’t always provided directly to the method being called - it may be some external system state (like a database record) that needs to be prepared in your test setup.  

For instance, consider a [`Command`](https://github.com/jbogard/MediatR/wiki#basics) that’s responsible for deleting a record from the database.  It’s very simple to write a test that assumes an empty database, insert a single test record, runs the `DELETE` command, then asserts the target table is empty.  But what if your `DELETE` command neglected to include a `WHERE` clause? The test is green, but you’ve got a major bug. Coverage isn’t about what lines of code get hit while the test is running, but whether the test is meaningfully demonstrating the behavior of the feature.
