---
layout: default
title: Formatting
parent: Pull Requests
grand_parent: Development Guidelines
nav_order: 1
---

# Formatting Pull Requests
{: .no_toc}
Pull Requests are a communication tool for discussing the completion of work items, understanding their changes, and integrating them with the product.  Submitters must format the request clearly in order to communicate all of the relevant information, facilitate a productive conversation, and ensure a smooth review process.

1. TOC
{: toc}

###  **DO** provide an informative description to guide the reviewer through your changes
{: .text-green-100 }
A good Pull Request includes a description that is informative to the reviewer.  Merely entering the name of the feature, or simply providing a list of commit messages misses an opportunity to convey context and intention to the reviewer. A good description highlights three aspects of the work being submitted.

First, it describes what capabilities the changes add to the system.  When describing the capabilities, consider your audience. If this is a user facing feature, describe what the user can do now.  If the work is largely unseen by end users, describe the change for a more technical audience.

Second, it highlights any new architectural decisions that were made and describes why they were made. If ‘architecture’ is defined as “the decisions which are hard to change”, the reviewer needs to be made aware of the consequences of approving the Pull Request.

Third, it highlights any unexpected obstacles encountered during development, and how they were addressed. This level of detail can help a reviewer to avoid being surprised or distracted by a complication encountered.

### **DO** provide specific instructions regarding how to manually test the changes
{: .text-green-100 }
For any change more than the trivial, it is a good idea for the reviewer to pull down the changes to their local machine during the review process. This allows the reviewer to step through the code base more easily during their review and to manually test the changes for themselves.

It may not be obvious to the reviewer how to test a particular feature or how to configure upstream data to properly create a test scenario for a given feature. If this is a possibility, provide the reviewer with instructions on how to prepare the environment and verify results.

### **CONSIDER** using a Pull Request Checklist
{: .text-yellow-300 }
As a team works together on a project, patterns in Pull Request feedback will begin to emerge to reviewers. [Checklists](http://atulgawande.com/book/the-checklist-manifesto/) are a great way to ensure common feedback items that prevent a Pull Request from being accepted are addressed, before the Pull Request is submitted. Modern tools such as Azure Repos, BitBucket and GitHub provide templating functionality for Pull Requests which can include a predefined checklist for developers to consider prior to submitting Pull Requests for their work. Checklists will vary greatly from project to project and should be tailored to the specific needs of a team and environment. Here is an example of a checklist used for an actual client project.

```
* [ ] Do I have appropriate unit testing?
   * [ ] Were there existing tests that I modified to match the feature change?
   * [ ] Are there new tests that I need to write to show the new feature working?
   * [ ] If you wrote tests that assert data changed on an object, are you properly setting values on the object before its changed?
* [ ] Did I manually test the feature(s)?
   * [ ] Did you happy path test the feature while also making sure to test non-happy paths? For example, if adding new fields to a form, making sure to test that it accepts valid values in the fields, while also testing that it rejects invalid values
   * [ ] Did you test items that are associated with it to ensure they all still work as they should? For example, if you made changes to the edit page for a contract, did you make sure the view page for the contract displays anything new that you added correctly and also if that contract data gets pulled in on a different page, that the changes you made show up on the related page?
* [ ] Did I confirm functionality of the feature to the assigned ticket*
* [ ] Is there whitespace and/or new line ending noise? Did you mix adding in tabs when the existing file had spaces? Diff tools like Beyond Compare and IDE’s like Visual Studio can be configured to* this so you can know before you create and push up a commit
* [ ] Do my variable names make sense?
* [ ] Is my feature branch up to date with changes from the destination branch I am merging it into?
* [ ] Is there any special scenarios I should make the PR reviewer aware of by documenting it in the PR description?
* [ ] Do all my commits start with the ticket number to link back to the ticket I made these changes for?
* [ ] If any new commits are pushed up after the PR is opened, has everything in this checklist been run through again?
```

The Markdown text can be pasted into the configuration for Pull Request templates in all three major source control providers.  In [Azure Repos](https://azure.microsoft.com/en-us/services/devops/repos/) and [GitHub](https://github.com/), the `* [ ]` will turn into a checkbox list that can be ticked off as the checklist items are completed.