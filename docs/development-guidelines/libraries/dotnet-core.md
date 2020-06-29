---
layout: default
title: .NET Core
parent: Libraries
grand_parent: Development Guidelines
nav_order: 5
---

# ASP.NET Core / .NET Core
{:.no_toc}

1. TOC
{:toc}

### **DO** favor .NET Core / ASP.NET Core over their predecessors
{: .text-green-100 }
.NET Core is intended to replace the .NET Framework. Think of it as a cross-platform rewrite of the .NET Framework, though one where breaking changes were allowed. As long as they both exist, they are distinguished as “.NET Core” (versions 1-3.x) and “.NET Framework” (versions 4.x). [When the rewrite is complete, .NET Core will be renamed simply “.NET” with versions starting at 5, and .NET Framework 4.x will simply fall out of support](https://devblogs.microsoft.com/dotnet/introducing-net-5/). Because the two platforms are planned to resolve in this way, we’ll experience less upgrade pain in the future by favoring .NET Core whenever possible.

Similarly, ASP.NET Core is the only ASP variant under active development. Whether targeting .NET Framework or .NET Core, favor ASP.NET Core over older versions of ASP.NET whenever possible. Because ASP.NET Core is the only ASP.NET long term, we’ll experience less upgrade pain in the future by favoring ASP.NET Core.

### **DO** target .NET Core for new Projects in a Solution
{: .text-green-100 }
Begin new projects by targeting .NET Core instead of the .NET Framework. It is easier to convert from .NET Core to .NET Framework once you run into a concrete obstacle than to convert in the other direction, and .NET Framework is meant to be replaced by .NET Core moving forward.

There are a few ways you may be forced to target the .NET Framework, though they all essentially boil down to the same thing: your code depends on a thing that in turn depends on the .NET Framework:

- You may depend on some class or method that does exist in the .NET Framework but simply does not exist in .NET Core. In some cases, this situation improves as each .NET Core release includes more of the familiar .NET Framework ‘surface area’.
- Some .NET Framework concepts simply will not ever be ported to .NET Core, and in these cases people are expected to solve their problems in a different way. AppDomains will never exist in .NET Core, but you’re probably better off working with separate processes and interprocess communication anyway. WCF will never exist in .NET Core, but you’re probably better off working with a simpler communication scheme anyway. These omissions are usually a matter of the underlying technology being either too Windows-specific or otherwise deprecated as problematic.
- You may depend on a NuGet package or an otherwise-distributed DLL that itself has not yet been ported to work on .NET Core. Some libraries may never be ported, and in those cases you may try first to find a comparable alternative library, so that you can continue working on .NET Core. For example, the ‘Should’ assertion library will likely never work on .NET Core, so we favor the quite-similar ‘Shouldly’ library, instead.
- You may depend on some other Windows-specific technology that has no .NET Core support. MSMQ for NServiceBus is specific to the .NET Framework.
Once a Project in your Solution changes to target .NET Framework, expect that decision to cascade through all dependent projects. It’s natural for Framework projects to reference each other, and it’s natural for Core projects to reference each other, but you can run into problems attempting to build a Solution of mixed projects.

### **DO** fall back to full .NET framework when necessary
{: .text-green-100 }
As .NET Core is not a complete rewrite of .NET Framework, and because you may depend on a library which has not been updated to support .NET Core, you may find yourself simply having to target the .NET Framework. Until the .NET ecosystem catches up, you may have no choice but to fall back to targeting the .NET Framework.

### **DO** work with the existing target framework when technology upgrades are not feasible
{: .text-green-100 }
A well-established project may already target the full .NET Framework or even an older version of MVC pre- ASP.NET MVC Core. Upgrading to ASP.NET Core running on .NET Core is a large and costly upgrade with little user-facing value, so although we may recommend a client keep up to date with modern technologies, we have to consider the practical costs and benefits when deciding where to focus our efforts. An in-place upgrade of an established application is likely not the best place to embrace this change.