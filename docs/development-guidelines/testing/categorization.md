---
layout: default
title: Categorization
parent: Testing
grand_parent: Development Guidelines
nav_order: 6
---

# Categorization of Tests
{: .no_toc}

1. TOC
{: toc}

### **DO** write tests at a useful level of granularity
{: .text-green-100 }
Should my “Unit” under test be a single class, a single method, a single line of code?

Rather than caring about a textbook ideal of what constitutes a “unit”, focus on two things:

* Does the test provide me with useful information, both when it passes and when it fails?
* Does the test give me the confidence to make change?

If your tests are so granular that they are annoyingly brittle, pull back up to a slightly higher level. If it still gives useful feedback and the confidence to make change, we shouldn’t care that “That’s not a unit”. Sure, it’s not a unit, but saying so isn't useful as it doesn't drive us toward a better decision.

### **AVOID** “arguing definitions” around the word Unit
{: .text-red-300 }
Developers easily focus too much on whether or not a test is a “Unit” test, leading to writing tests that fit some textbook ideal while providing little or negative value. People will argue over whether the unit being tested is really a unit, and even attach a value judgement that a test is good or bad if it meets or fails to meet that ideal ‘unit’.

Is a method a unit? A class? A line of code? A group of closely related classes? In most cases, this precise of a definition adds little value. However, it is often worth distinguishing the difference between a Unit and Integration test.

A Unit test is a test that does not “go out of process”. A test goes out of process if the running test or Subject Under Test (SUT) communicates with an outside system such as a database or the filesystem. A test remains in process if it’s simply manipulating objects in memory or performing calculations.

An Integration test, then, is any test that does go out of process. If your test or SUT touches the filesystem, queries a database, sends an email, `POST`s to a running API… it’s an integration test.

Again, though, we don’t really care about that, especially when deciding whether or not a test is “good”. For that, we need only focus on two things:

* Does the test provide me with useful information, both when it passes and when it fails?
* Does the test give me the confidence to make change?
