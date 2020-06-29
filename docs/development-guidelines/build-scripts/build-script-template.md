---
layout: default
title: Template
parent: Build Scripts
grand_parent: Development Guidelines
nav_order: 3
---

# Build Script Template
{:.no_toc}

- TOC
{:toc}

### **CONSIDER** using the build script template as a starting point for your project
{: .text-yellow-300 }
The [template](https://github.com/HeadspringLabs/sample-build-script) is a starting point for your build scripts that gives you the basic skeleton and some useful helper functions.

Copy these files to your project to start using it:

- `build/general-helpers.ps1`
- `psake.cmd`
- `psakefile.ps1`
- `setup.cmd`
- `setup.ps1`

Remove the items you don’t need from `setup.ps1` and add the items you do need. Then update `psakefile.ps1` with your customizations.

## Property Structure

Properties in Psake build on top of each other, and you can override these on the command line or in subsequent parts of your build script.

```powershell
properties {
   $configuration = 'Release'
   $version = '1.0.999'
   $owner = 'Headspring'
   $product = 'Sample Build Script Application'
   $yearInitiated = '2018'
   $projectRootDirectory = "$(resolve-path .)"
   $publish = "$projectRootDirectory/Publish"
   $testResults = "$projectRootDirectory/TestResults"
}
```

- **configuration**: either ‘Debug’ or ‘Release’ depending on what your build script needs (typically ‘Release’)
- **version**: a default version to apply to the assembly when running outside of a CI build (can be overridden on the command line)
- **owner**: the company that owns the copyright of the software
- **product**: the name of the software product that will be set in the assembly metadata
- **yearInitiated**: the oldest year to use for the copyright notice
- **projectRootDirectory**: a calculated path at startup to use for absolute paths
- **publish**: the directory where published binaries and files go
- **testResults**: the directory where published test reports go

Tasks in `psakefile.ps1`

These are the tasks in the sample build script and what they do:

- **default**: if you try to run Psake without a target, this one is used
- **CI**: run the sequence of tasks expected for a CI build and publish
- **Rebuild**: Clean up and recompile the application
- **Info**: display information about the runtime environment
- **Test**: run unit tests
- **Compile**: build the application
- **Publish**: publish the top level projects of the application
- **Clean**: clean up the binaries
- **? / help**: call the Psake internal function to write the task details

Tasks / functions in shared global helper script

The shared global helper script is the template script has a few helper functions:

- **Get-Copyright**: generate a copyright message based on project properties
- **Publish-Project**: publish a project into the appropriate sub folder in the publish folder
- **Set-Regenerated-File**: replace a file’s contents if the new contents are different from the old
- **Remove-Directory-Silently**: delete all the files in a folder and ignore any errors

Tasks / functions in project-specific helper script

If you have custom functions that you need, you have two options:

### **CONSIDER** putting helper functions in your psakefile.ps1 when they are few and short
{: .text-yellow-300 }
Simple short functions are easier to keep in the main `psakefile.ps1` to avoid needing to create a separate file.

### **CONSIDER** putting helper functions in a separate project-specific `build/custom-build-helpers.ps1` file when they are more complicated
{: .text-yellow-300 }
When the helper functions are larger and more complicated, considering putting them into a separate file to keep the main `psakefile.ps1` concise.
