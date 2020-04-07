---
layout: default
title: Reviewing
parent: Pull Requests
grand_parent: Development Guidelines
nav_order: 3
---

# Reviewing Pull Requests
{: .no_toc}

The Pull Request review is the last line of defense for code entering the mainline branch. The reviewer has the responsibility of holding the developer accountable for maintaining team development standards, sound architectural decisions, consistent coding practices and the completeness of the requested functionality. Below are some general guidelines to follow during the review process.

1. TOC
{: toc}

### **DO NOT** fully review a pull request that has glaring problems
{: .text-red-300 }
Feedback that can get back to the developer quickly is useful to avoid having to switch contexts.  When a reviewer sees a notification about a PR they should consider quickly reviewing the Pull Request in the Bitbucket or GitHub GUI first for any glaring misses, such as:

* No ticket referenced
* No summary or overall description of what the PR does
* Badly named variables, methods, classes, etc.
  * Include following conventions such as inner classes for handlers or child collections on domain models
* Missing tests
* Broken build
* Any other checklist items that were obviously missed

In these cases, the reviewer should write a short response and send the Pull Request back to the submitter to resolve.  It is best not to waste time  completing a thorough review if the chance of merging the Pull Request is already quite low.

### **DO** update from the target branch
{: .text-green-100 }
Make sure the branch is fully updated with the mainline branch such as “master” or “develop” prior to starting a review. The submitter should be expected to do this as well, prior to submitting the Pull Request. However, doing this as a reviewer ensures that any changes merged since the Pull Request was submitted are incorporated in the review.

### **DO NOT** rebase a Pull Request branch while it is being reviewed
{: .text-red-300 }
Updating a branch against a mainline using rebase rewrites the history of the branch under review. This will cause the submitter to be completely out of sync with the remote repository.

### **DO** prioritize reviewing the Pull Request file change diff first
{: .text-green-100 }
Reviewing the Pull Request file change diff first allows the reviewer to stay within the context of the source control tool. The diff view allows the reviewer to focus on more obvious feedback items such as whitespace, naming, and general structural before committing to switching contexts by pulling down the code to their local machine.

### **CONSIDER** reviewing individual commits to more easily build a mental model of the changes
{: .text-yellow-300 }
Reviewing individual commits within a Pull Request can help the reviewer establish a mental model of the changes, making a Pull Request easier to digest. Reviewing individual commits can also speed up the review of follow up changes based on initial Pull Request feedback. Additionally, examining the commit history of a Pull Request can give the reviewer insight in to how the developer approached the problem which may uncover valuable coaching opportunities.

### **DO** pull the Pull Request branch down locally and test both “happy” and “error” paths
{: .text-green-100 }
For anything other than the most trivial Pull Requests it is important for the reviewer to pull the code under review down to their local machine. It’s much easier to review changes within the context of the entire code base and with the availability of automated navigation tools provided by the IDE.

The reviewer should verify that the changes accomplish the requirements of the work item through manual testing. If the Pull Request does not encompass the entire body of work for a work item, the reviewer should have a clear understanding of what is left to be completed. If the reviewers system does not have the data to test some conditions, such as a paging implementation when there is not enough data in a local database, the reviewer should mock the scenario by providing placeholder data or manually updating the database with quasi-realistic data.

The reviewer should take detailed notes of testing scenarios that fail and provide that information to the Pull Request submitter in the review.

### **DO** verify database updates apply correctly
{: .text-green-100 }
The reviewer should verify that any database updates apply correctly on their local environment. It is important to test both update and rebuild scenarios and to check migration scripts for possible timeouts or gotchas based on exact data usages.

### **DO** execute the entire test suite locally
{: .text-green-100 }
Both submitter and reviewer should run the entire unit and integration test suite locally.

### **DO NOT** reject / close a pull request before talking with the submitter
{: .text-red-300 }
Outright rejecting a PR is an uncommon event, so it is important for the reviewer to be mindful when doing it.  After a feedback conversation, it is always better to ask the submitter to amend the Pull Request by editing or pushing, or to retract it and resubmit  the changes in a new PR.  As a reviewer, it is always better to let the submitter take the required actions and resubmit or flag the Pull Request as ready to be reviewed again.

### **AVOID** deleting branches right after merging pull requests
{: .text-red-300 }
When closing pull requests after merging, both GitHub and Bitbucket have options for deleting the source branches.  If you are maintaining multiple environments at slightly different versions, you will want to keep the source branches intact for a period of time (depending on your project) in case you need to back out changes and create an interim version.  GitHub and Bitbucket behave differently when you close the source branch: GitHub will keep it around for some time and allow you to undelete it, Bitbucket does not provide the same ‘soft delete’ functionality.

