---
layout: default
title: Test Suite Health
parent: Testing
grand_parent: Development Guidelines
nav_order: 3
---

# Test Suite Health
{: .no_toc}
Keep the build green. We too-easily train ourselves to stop caring about tests when they are flaky, when we allow problematic tests to keep the build red, etc.

1. TOC
{: toc}

### **AVOID** skipping tests
{: .text-red-300 }
Generally, avoid skips since they can be a crutch to allow failing or irrelevant tests to live on. You may have a legitimate reason to skip a test, such as one that’s written knowing that a dependency will be ready for it by a certain time. If you do skip a test, ensure you will unskip it via a ticket and a well defined trigger or timespan. Otherwise it’s too much like a `TODO` and will always sit there.

### **AVOID** removing a failing test
{: .text-red-300 }
Removing a test needs to be done with extremely deliberate reasoning. You might be opening the door to a regression or unintended behavior change. Is the test failing because it is no longer applicable? Can the test be meaningfully updated to meet the new intended behavior?

### **DO NOT** react to a failing test by taking the shortest path to make it green
{: .text-red-300 }
“Curing the symptom” in reaction to a test failure is a common mistake among people first learning how to work with tests. When a test fails, seek evidence. Understand why it's failing and what that is telling you. Any part of the test or SUT might be the actual culprit.

A failing test may legitimately deserve to be changed. For instance, a legitimate improvement to the feature under test may force you to reconsider old assertions that are no longer desired. Change existing tests with caution, precisely because they are telling us about a behavior change.
