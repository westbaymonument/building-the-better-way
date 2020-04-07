---
layout: default
title: Source Control
nav_order: 5
parent: Development Guidelines
---

# Source Control
{:.no_toc}

Headspring's default source control system of choice is Git. Git is a distributed source control system which differs from centralized systems in that it maintains a local copy of the entire repository on each users local environment and most operations are executed locally. Some of the many benefits of this design include fast and efficient branching and merging, as well as the ability to operate without having to connect to a centralized service. This document captures general guidelines regarding how to make the most effetive use of Git when executing projects.

* TOC
{:toc}

## Source Control Management System Bridges

### **CONSIDER** using a bridge between Git and other Source Control Management Systems
{: .text-yellow-300 }
In cases where another established source control system is being used, it may be beneficial to use a tool to bridge Git with another system. For example, [git-svn](https://git-scm.com/docs/git-svn) allows for bidirectional operations between a Subversion repository and Git. This approach provides developers with the benefits of using Git locally as well as the ability to keep a local repository in sync with a centralized Subversion repository. [git-tfs](https://github.com/git-tfs/git-tfs) provides similar functionality for Team Foundation Server repositories.

## Commits

A commit is the Git operation which records changes to the local repository. Commits are tracked by Git as a series or history of changes to the repo. Though this operation is simple, there are many things we can consider in order to maximize the benefits of this design.

### **DO** write meaningful and informative commit messages
{: .text-green-100 }
With each commit a developer can attach a message describing the changes being introduced. This features provides the opportunity for storing documentation of the changes to the system with the source code itself. A good commit message describes _why_ the changes under `commit` are being introduced. This is fundamentally different, and far more useful to future developers, than discribing _what_ changed. _What_ changed can easily be seen by evalueating the `diff` between two commits which is supported by most third-party Git clients and hosted Git repositories such as GitHub. A commit message that describes the _why_ of the change focuses on what the user is now capable of doing and/or how the change fits in to a larger scope of work.

### **CONSIDER** adding a more detailed explaination of the changes in the commit message
{: .text-yellow-300 }
There is no practical limit on the length of a `commit` message so there is no need to be breif. The expected format for a message is to provide a short, 70 characters or less, summary as the first line, followed by a blank line, followed by a more in depth description of the changes. Some third-party Git clients provide two fields to facilitate this approach.

```text
Redirect user to the requested page after login

Users were being redirected to the home page after login, which is less useful
than redirecting to the page they had originally requested before being
redirected to the login form.
```

This format can be critical in some git clients and tools as the blank line is used to differentiate between the summary and body of the message.

### **CONSIDER** adding ticket identifiers to the commit message
{: .text-yellow-300 }
There are cases where it is useful to be able associate a change introduced by a single `commit` to a larger set of work such as a Pull Request or the documentation captured in a project management tool. To faciliate this it can be very useful to include a ticketing system identifier in the `commit` message. This can be done either as the first element of the message or within the detailed explaination of the changes.

```text
JIRA-1337 Redirect user to the requested page after login

...
```

```text
Redirect user to the requested page after login

(JIRA-1337)

...
```

### **DO** make small, focused commits
{: .text-green-100 }
Each commit should capture a small cohesive set of changes. Keeping commits small and focused allows developers establish a history of changes that describe how the work was implemented, and easily revert changes if necessary. Additionally, small and focused commits provide the opportunity to document each change with a detailed message that describes _why_ the change is being introduced.

### **DO** commit frequently and push often
{: .text-green-100 }
Commiting frequently insures that there is at least a local back up of the changes being introduced while pushing often insures there is a remote back up. Frequent pushes and pulls to remote repositories also allow developers to keep their work in sync with other changes and provides visibility in to the work being done for the rest of the team.

### **DO** isolate formatting, whitespace and clean up changes in their own commits
{: .text-green-100 }
It's exponentially more difficult to understand the functional changes introduced by a commit when the commit includes whitespace, reformatting and clean up work. When displaying a `diff` of two commits, remote repositories such as GitHub or local clients surch as Source Tree have no way to differentiate the meaningful changes from the trivial. Keeping formatting changes separate, allows developers to view a set of changes commit by commit and know right away which ones to focus on.

### **CONSIDER** amending a previous commit
{: .text-yellow-300 }
In situations where a change should be included with previously commited work, consider using the `--amend` flag. This flag will effectively add the change to the previous `commit` by replacing the previous commit with a new one that incorporates both sets of changes.

### **CONSIDER** using interactive rebase to clean up commit history
{: .text-yellow-300 }
Git's Interactive Rebase feature essentially allows the developer to rewrite the commit history of a given branch. This can be useful in cases where multple commits should be combined or commit messages need to be clarified. Interactive Rebase is an effective tool that developers can use to ensure their commit history is clean, concise and tells a story of how a feature was implemented.

### **CONSIDER** using git hooks to automated repetative tasks on each commit
{: .text-yellow-300 }
Git Hooks allow developers to automate repetative tasks for each commit. This functionality can be used to automate any number of tasks. One example of a useful automation is using a git hook to prepend the ticket number to each commit message based on a branch naming convention. Other examples include enforcing spellcheck requirements on changes and linting of code.

## Branching

### **DO** have a branching strategy
{: .text-green-100 }
It is important to have a branching strategy that is used consistently across a team and a project. There are numerous published strategies for accomplishing this including git flow. Headspring's default branching strategy is to create feature branches off of master or a develop branch. This keeps all changes related to a single feature on the same branch and minimizes the need to synchronize multiple higher-level branches. We typically discourage branching off of feature branches to minimize the potential for conflict. Additionally, feature branches make it easier to review all changes related to a specific ticket, gives developers the freedom to experiment without destabalizing the time, and ensures the stability of the integration branch at all times facilitating continuous deployment.

### **DO** have a branch naming strategy which includes the ticket number and a brief summary
{: .text-green-100 }
It is important to establish a branch naming strategy across a team and a project. A simple and consistent naming strategy makes it much easier for developers to reference branches and execute commit operations from the command line. As mentioned previously with git messages, it's useful in many circumstances to be able to tie changes back to a project management ticket. Starting each branch name with the relevant ticket identifier allows anyone to quickly associate the branch with correct ticket. Headspring's default branch naming strategy is as follows:

```text
[Project Identifier]-[Ticket Identifier]-[Description]
```

The `Project Identifier` is a capitalized project identifier established in our default project management tool, Jira. The `Ticket Identifier` is the integer identifier for the ticket being developed against and is established by Jira as well. The `Description` is a very brief description of the feature written in all lower case and separated by hyphens.

```text
Example:

TECH-1234-user-login-page
```

This format allows anyone to easily identify the corresponding ticket for the work being done on a branch. The descriptive text quickly conveys what is being done and the use of lower case text and hyphens allow the branch name to be typed efficiently.

## Rebase and Merge

There are two ways to join two or more development histories together with git. Merge incorporates the changes from one branch in to another by replaying all of the commits that took place since the two branches diverged. The changes are recorded as a single new commit on the destination branch. Rebase incorporates the changes from one branch in to another by replaying all of the commits individually as new commits on the destination branch. These two strategies both have tradeoffs that are important to consider.

### **CONSIDER** rebasing when pulling from upstream
{: .text-yellow-300 }
If a developer has local commits on a branch that have not been pushed to the remote repository and changes from the remote need to be pulled to update the local branch, consider executing a pull with rebase (`git pull --rebase`). A pull with rebase will replay the local commits that haven't been pushed after pulling down the latest commits from the remote repository. This will help maintain a clean local commit history rather than introducing an uncecessary merge commit.

### **DO NOT** rebase a branch once it has been pushed to a remote repository without communicating
{: .text-red-300 }
As mentioned previously, rebasing effectively rewrites history. Rebasing a branch that has been pushed will require a forced push to update the remote branch. This could have the unintended consequence of overwrting other work that has been done on the remote branch. Once a branch has been pushed, there is no guarantee that someone else hasn't pulled down the same branch and made additional changes. Because of this, it's important to communicate with the team that this needs to take place and to make sure there are not unintended consequences.

### **DO** rebase locally
{: .text-green-100 }
Interactive Rebasing provides the opportunity to clean up commit history, reorganize commits and adjust commit messaging. Developers should feel free to take advantage of rebase operations locally until their branch has been pushed to a remote repository.

### **AVOID** force pushing without the 'with-lease' extension
{: .text-red-300 }
In the case where a developer finds themselves in need of executing a forced push, use the `--force-with-lease` option as a safety net. `--force-with-lease` will verify that no other changes will be overriden and lost prior to executing the force.

## Tools

There are many third-party tools available to help maximize the benefits of git. Though we encourage familiarity with using git from the command line, the third-party tools listed below add extremely useful functionality to our git workflows.

### **CONSIDER** using Sourcetree or Fork as a Git client
{: .text-yellow-300 }
Both of these tools are commonly used by Headspring engineers.

### **CONSIDER** using P4Merge or Beyond Compare as a merge tool
{: .text-yellow-300 }
Both of these tools provide 3-way merge capability and are very useful when working through merge conflicts.

### **CONSIDER** using Git Bash or Posh-Git as a command line tool
{: .text-yellow-300 }
Git Bash provides a bash environment on Windows while Posh-Git adds Git context information to a Powershell console.

### **CONSIDER** using Git Extensions for explorer context menu extensions
{: .text-yellow-300 }
Git Extensions provides context menu Git functionality.
