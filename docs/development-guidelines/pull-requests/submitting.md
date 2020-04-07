---
layout: default
title: Submitting
parent: Pull Requests
grand_parent: Development Guidelines
nav_order: 2
---

# Submitting Pull Requests
{: .no_toc}

The work of creating a good Pull Request begins while the developer is writing code. For example, a clean, well thought out commit history allows the reviewer to examine each commit individually and build up a mental model of how the feature was implemented as they progress through the commit history. Treating each commit message as form of documentation allows the reviewer and future developers to understand the reasoning behind each change.  Ensuring consistent and meaningful method and variable names maximizes the readability of the code base. Below are some general guidelines to follow during development that can help ensure a smooth Pull Request review.

1. TOC
{: toc}

### **DO** evaluate the pull request with the reviewer in mind
{: .text-green-100 }
When submitting a Pull Request, look at the diff of the code on GitHub, Bitbucket, or Azure Repos through the lens of the reviewer.  If the list of files changes is excessive, it will be more difficult for the reviewer to work through the changes in a timely manner. In cases where a single change impacts many files, such as renaming an object, isolate the change to a single commit so that the reviewer can examine the other changes more easily.

### **CONSIDER** using an interactive rebase to clean up commit history
{: .text-yellow-300 }
As mentioned previously, a clean, well thought out commit history allows the reviewer to examine each commit individually and build up a mental model of how the feature was implemented as they progress through the commit history. It is unrealistic to expect any developer to be capable of perfectly planning every commit. With that in mind, [interactive rebase](https://git-scm.com/book/en/v2/Git-Tools-Rewriting-History) is a great tool to use for restructuring commit history or cleaning up commit messages.

### **DO** ensure only changes related to the work item are included
{: .text-green-100 }
Prior to submitting a Pull Request, double check the work assignment to verify that the changes being submitted fulfill all of the requirements expected and nothing more. Just as developers follow the [Single Responsibility Principle](http://en.wikipedia.org/wiki/Single_responsibility_principle) in code, Pull Requests should be focused on a cohesive set of changes. Any work that doesn’t belong should be moved off onto a separate branch and Pull Request. This helps ensure that Pull Requests are small in size and focused so that the review process can be completed quickly.

### **DO NOT** commit erroneous whitespace changes
{: .text-red-300 }
Unnecessary whitespace changes add noise to the file diff and act as small speed bumps for the reviewer as they move through the Pull Request. If the whitespace changes are a clean up due to a previous mistake or a change in coding standards, consider making the changes in their own commit or a separate branch and Pull Request.

### **DO** ensure that function and variable names are consistent, clear, and spelled correctly
{: .text-green-100 }
Ensuring consistent and meaningful method and variable names maximizes the readability of the code base. [ReSharper](https://www.jetbrains.com/resharper/) has spell check tool built in that can be turned on and used to help resolve spelling issues within the IDE. Abbreviations and domain specific terminology can be added to a configuration file for the spell check tool to ignore.

### **DO NOT** submit a Pull Request with commented out code
{: .text-red-300 }
While the branch is a work in progress, it is understandable that code may be temporarily commented out.  However, compared to future developers who will have less context, the author of the change is the person in the best position to know whether that code should stay or go and should make the decision before submitting the Pull Request.

### **CONSIDER** turning comments into aptly-named functions
{: .text-yellow-300 }
Code should be developed with a focus on readability. This allows a code base to be self-documenting, reducing the need to explain functionality to future developers in the form of code comments. Turning a code comment in to an aptly-named function can be a useful approach for avoiding code comments. For example, a smart search algorithm that checks if the user submitted a number vs alphanumeric to determine whether to search by number or name could be refactored in to separate methods named `FindUserById` and `FindUserByName` for clarity.

### **DO** update the branch from the latest upstream branch
{: .text-green-100 }
Be sure to update the branch from the latest upstream, such as master, using either a merge or rebase strategy as defined by the team. This will help avoid the reviewer from having to reject the PR due to conflicts and ensures the changes being introduced are compatible with the latest version of the code base.

### **DO** ensure that database scripts and migrations work against the previous upstream version
{: .text-green-100 }
Ensure that any database scripts work with both an incremental deployment and full rebuild. This will help verify that all necessary local database changes were included in the migration scripts and will not fail on the reviewers environment.

### **DO** manual testing of “happy” and “error” paths
{: .text-green-100 }
Automated unit and integration tests are paramount for a healthy code base but manual testing is equally important and the entire teams responsibility. Prior to submitting a Pull Request, execute the feature locally to test that the functionality is working as expected.  Be sure to test error paths in addition to happy paths. For example, try to include things such as purposely throwing an exception, or closing the database down to see how the client might respond to an unexpected `500` result.

### **CONSIDER** submitting a draft Pull Request to start a conversation
{: .text-yellow-300 }
The draft Pull Request tool can be a great way to gather feedback for in-flight work that isn’t ready to be merged in to a mainline branch. Some tools — like [GitHub](https://github.com/) and [Azure DevOps](https://azure.microsoft.com/en-us/services/devops/?nav=min) — provide first class support for draft Pull Requests. This can be manually done as well by establishing a team convention such as prepending the title of a Pull Request with ‘FEEDBACK ONLY’. When submitting a draft Pull Request be sure to provide clear and specific guidance to the reviewer regarding the state of the code and the items to provide feedback on.