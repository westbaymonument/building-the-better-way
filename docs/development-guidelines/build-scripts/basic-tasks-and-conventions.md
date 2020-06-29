---
layout: default
title: Basic Tasks
parent: Build Scripts
grand_parent: Development Guidelines
nav_order: 1
---

# Basic Tasks and Conventions
{:.no_toc}

Build scripts facilitate automation. Automating tasks reduces human touches and the potential for mistakes. Continuous improvement processes utilize automated processes to ensure that projects build consistently.

- TOC
{:toc}

### **DO** have a consistent naming and casing convention for Psake tasks and PowerShell methods
{: .text-green-100 }
Tasks are created in the `psakefile.ps1` file. Naming and casing conventions are whatever the team decides. The PowerShell guidance convention is capitalized Kebab-Case. We also like regular kebab-case. You do you, but maintain consistency throughout the file.

### **DO** create tasks for ubiquitous development activities that all projects will consume
{: .text-green-100 }
In addition to building the solution, a proper build tool automates day-to-day developer activities with a menu of commands - cleaning the solution directories (\bin\, \obj\, etc.) of past build artifacts, running tests, migrating the database to the latest version or refreshing sample data, starting services on which the solution depends to function, etc. Therefore, build scripts give developers an easy way to browse available tasks and advertise their purpose.

## Documentation Inside a Build Script

Psake offers a PowerShell StringLiteral `-Description` as a parameter to hold task descriptions. Psake writes out the description for all tasks when passed the built in `-Docs` or `-DetailedDocs` parameters. The `-Docs` parameter prints a list of tasks in simple rows. `-DetailedDocs` iterates through the `Task` objects and prints out the results of the `Task` as a PowerShell object, with each property on a separate line.

### **DO** document tasks and include detailed descriptions of their purpose.
{: .text-green-100 }
The following Build task example has a Description that is displayed when a user executes `Invoke-Psake -docs`.

```powershell
task Build -Description "Builds the solution" {
    dotnet build --framework $framework --configuration $configuration --no-logo
}
```

Psake simplifies task execution using an Alias, as an alternative to typing Tasks with long names.

```powershell
task IntegrationTests -Alias itest -Description "Runs integration tests" {
    # ...
}
```

```powershell
> Invoke-Psake itest

Running RunIntegrationTests...
```

### **DO** create terse but descriptive task names
{: .text-green-100 }
Task names should be immediately readable for context by new developers. For example: `Clean`, `Build`, `Test`, `Migrate-Database` versus `Delete-BinObjFolders`, `Build-Solution`, `Exec-Roundhouse`. For day-to-day tasks, Migrate-Database does the job.

### **CONSIDER** using the name Verb-Noun format when naming Tasks
{: .text-yellow-300 }
PowerShell naming convention uses a verb-noun format. Names like `Run-IntegrationTests`, `Migrate-Database`, `Populate-SampleData` communicate the operation and what it is operating upon.

### **AVOID** excessively long task names
{: .text-red-300 }
### **CONSIDER** adding `-Alias` for longer, frequent task names
{: .text-yellow-300 }
Forcing developers to type `Execute-CompanyMandatedReductionProcedure` for a commonly used task will quickly become tedious, even if it is accessible in the buffer. It is best to provide a shorter alias for necessarily long task names. Keep task names under thirty characters, if possible. Maybe the team knows this particular task as the “squash” task. “Squash” would make a good candidate for an alias. Use good judgement and utilize existing domain language without inventing new terms.

Although `Migrate-Database`, from above, is a good, terse task name, your users will appreciate it more if they only have to type `Migrate`.

### **DO** keep your scripts as platform-neutral as possible
{: .text-green-100 }
PowerShell (and the .NET Core SDK) run on both Windows and Linux, so your build script should be able to work with both operating system environments seamlessly. Keep in mind that you may not be running in a Windows OS and you may not be running with administrative privileges.

### **DO** use forward slash for your path separation
{: .text-green-100 }
When defining paths in PowerShell strings, use the forward slash “/” as a path separator instead of backslash “\”.  This will translate properly on both Windows and Linux and you can do path concatenation and separation universally. Also, use the Path APIs in your scripts instead of manually searching in a string for path separators.

### **DO** assume file and folder names are case-sensitive
{: .text-green-100 }
Linux filesystems are case-sensitive while Windows filesystems are not. This means that if you mix cases in a filename, you may end up with unexpected errors.

These filenames are not the same in Linux

```
Directory.build.props
Directory.Build.props
```

These two files mean the same thing in Windows and .NET Core SDK will pick up the files and use them the same. In Linux, however, the first form will be ignored entirely (without an error message), while the second form will be used as expected.

Similarly, if you are running executables as part of your build or referring to specific folders, the casing of the file or folder matters. You will later be able to test out your builds in a Docker container in case you are developing on a Windows machine and want to be sure your builds will work. The current Docker images for the .NET Core SDK 3.1 include PowerShell 7 in the Linux and Windows platforms.
