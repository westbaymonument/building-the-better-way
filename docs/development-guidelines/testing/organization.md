---
layout: default
title: Test Project Organization
parent: Testing
grand_parent: Development Guidelines
nav_order: 7
---

# Test Project Organization
{: .no_toc}
Here are some general guidelines for establishing test projects within your solution.

1. TOC
{: toc}

### **DO** Create one test project per tested application project
{: .text-green-100 }
We want our integration tests to mimic the deployed runtime as much as possible. Mixing the tests of multiple applications in a single test project causes quite a few issues. For instance, a Web Application and a Windows Service in the same solution may have substantively different TargetFramework values, substantively different IoC setup, etc.

### **CONSIDER** Further separating unit vs integration test projects
{: .text-yellow-300 }
Example: `TestedProjectName.UnitTests` and `TestedProjectName.IntegrationTests`.

You may wish to do this to isolate unit tests from making assumptions about integration-testing setup.

You may wish to do this in order to fail a build quickly, for systems in which the integration tests take a substantial amount of time to run. By running the inherently-faster unit tests in isolation first, you give the CI system a chance to stop early, without having to start the long integration test run, if we already know there are failing unit tests.

Most projects grow large, leading to slower builds during the integration test step, and it can be hard to split up Unit/Integration tests after the fact.

Be wary of the cost, though: the more projects a solution has, the slower the build times. A slow build can hinder productivity over the lifetime of a project.

If you choose this separation, adjust your namespace globbing pattern as necessary to split build server steps to Unit and Integration tests. Set UnitTests to run first as they are typically faster, so you can get earlier feedback in the build.
