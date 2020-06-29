---
layout: default
title: Build Scripts
nav_order: 7
parent: Development Guidelines
has_children: true
---

# Build Scripts
{:.no_toc}

A good build script supplies low-friction commands that new developers can use to immediately build a project. A single command -- usually, build, buildall, etc. -- performs all of the steps necessary for a first-time run. Adding more commands automates day-to-day development activities.

A build script is the introduction to initiating builds for a project. A build script should offer a set of tasks that perform an operation - cleaning binaries or other files from the solution directories, building the solution, running unit tests, migrating the database, or even running an entire set of tasks for a continuous integration build.

- TOC
{:toc}

## Psake Build Script Framework

### **DO** use PowerShell scripts for creating a build script
{: .text-green-100 }
Build scripts should be written in a shell for your operating system. In the case of .NET and Windows projects, write build scripts using PowerShell. PowerShell offers access to numerous cmdlets for Windows specific providers, or even consuming .NET libraries. PowerShell comes with Windows and has a low-friction installation. PowerShell is also available on Linux, as well as being the main scripting language for Azure DevOps.

### **DO** use Psake as your build script engine
{: .text-green-100 }
[Psake](https://github.com/psake/psake) (pronounced `sake` like the rice wine) is a build automation tool built in PowerShell that extends basic PowerShell scripting for task execution, documentation, and a task execution pipeline. Psake uses a dependency pattern that allows the construction of task actions that simply execute a sequence of other pre-built tasks. Conditional execution (“precondition”) is another option with Psake - ensuring certain requirements are met before a task executes. This is useful for preventing execution of tasks in certain environments.

```powershell
Task MigrateLocalDb -Alias localdb -Description "[LOCAL-ONLY] Migrates the database" -precondition { return Is-LocalHost() } {
    Migrate-db $connection_string
}
```

Psake’s main convention is having a main build script file `psakefile.ps1` in the root of the solution directory. All of the build’s tasks will reside in this file. Psake also accepts command-line arguments - Properties and Parameters.

**First Time Run**

The solution should present a setup command -- `setup.cmd` and `setup.ps1` -- that configures all of the one-time configuration that is required for builds to execute. This script applies any needed changes to the PowerShell configuration - such as trusting the PowerShell Gallery for downloads -- setting the appropriate execution level for remote scripts, and installs any required PowerShell modules.

### **DO** create an initial setup script that configures the machine for the first time run
{: .text-green-100 }
At minimum, you’ll need these lines (`setup.ps1`):

```powershell
# set PSGallery as trusted so we can install packages from there
Write-Host 'Trusting PS Gallery'
Set-PSRepository -Name "PSGallery" -InstallationPolicy Trusted
 
# Install PSAKE
Write-Host 'Installing PSake'
Install-Module -Name psake -Scope CurrentUser
```

This will ensure that you have Psake running and working. Version 4.9.0 is available at the time of this writing and works fine.

## When to Break From These Guidelines

### **AVOID** switching an established CI/CD process to Psake
{: .text-red-300 }
Use the conventions in this document, except when you shouldn’t. If you are working in an existing project with an established build pipeline and tooling around it, there is no need to inject Yet Another Tool™ to the process unless there are major defects in the existing process and switching will introduce significant improvements to developer productivity.

### **DO** consider the expertise of the maintaining team when proposing a build script
{: .text-green-100 }
Avoid creating a process to deploy the main product in PowerShell if the team responsible for maintaining the application doesn’t have any PowerShell expertise.

### **CONSIDER** breaking the guidelines in this document if the break will introduce considerable developer productivity and improve clarity
{: .text-yellow-300 }
If you do decide to break from the guidelines, document any “strange” behaviors or side effects that could be introduced from your changes. Generally the README file is a good place to denote these things.

### **CONSIDER** using built-in build steps in CI/CD tooling when the tool-based integration is faster or more feasible than build-script-based integration
{: .text-yellow-300 }
Some tasks in the build process require extra setup, licensing, or integration that is just faster or easier to integrate with the CI/CD tooling (Azure DevOps, TeamCity) rather than implementing the functionality from scratch in the build script. 

When code coverage during unit testing is a requirement, Azure DevOps and TeamCity have integrated capabilities that are a checkbox away (if you run your unit testing using their built-in steps) rather than setting up the incantations using a build script. For example, TeamCity provides built-in support for dotCover and Azure DevOps provides built-in support for code coverage in Visual Studio based test running (on Windows agents).
