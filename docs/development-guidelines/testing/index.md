---
layout: default
title: Testing
parent: Development Guidelines
nav_order: 2
has_children: true
---

# Testing Guidelines

Projects delivered by Headspring are expected to include a set of well-written automated tests that provide meaningful coverage of the code we deliver.  But what does that really mean? This document is intended to be a guide for what constitutes acceptable tests in a project, how to write them, and guidance on implementing common testing patterns we frequently see in the projects we deliver.

## The Importance of Testing

Tests are a valuable part of our software development toolkit, and one of the main reasons we can deliver high quality code at a rapid pace.

Tests allow us to do all of the following in an automated fashion:

* Validate that the code we write works as intended
* Refactor with confidence that we haven’t broken any old code
* Ensure integrations at code seams function as we expect
* Verify that we’ve effectively resolved previously identified bugs
* Add new features without breaking old ones

We test because it makes our work product for our clients better.
