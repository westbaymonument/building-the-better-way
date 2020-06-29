---
layout: default
title: Identity & IdentityServer
parent: Libraries
grand_parent: Development Guidelines
nav_order: 3
---

# Identity & IdentityServer
{:.no_toc}

1. TOC
{:toc}

## Recommendations
{:.no_toc}

### **DO** use ASP NET Core Identity when developing a NET Core application
{: .text-green-100 }
When you are not using an external identity system such as Google identities for logins, use ASP.NET Core Identity to set up the typical set of user-tracking tables and behaviors, so that you don’t have to reinvent them from scratch.

ASP.NET Core Identity is a membership system that adds login functionality to ASP.NET Core apps. See [Introduction to Identity on ASP.NET Core](https://docs.microsoft.com/en-us/aspnet/core/security/authentication/identity?view=aspnetcore-2.2&tabs=visual-studio)

### **DO** use .NET Identity when developing a .NET application
{: .text-green-100 }
If you're not working with ASP.NET Core, see Introduction to ASP.NET Identity.

### **AVOID** using .NET Membership when developing a .NET application
{: .text-red-300 }
.NET Membership has been replaced by .NET Identity.

### **DO** use IdentityServer
{: .text-green-100 }
We use [IdentityServer](https://identityserver.io/) for any non-trivial authentication needs. IdentityServer eases OAuth / OpenID Connect integrations in support of single sign-on, identity management, authorization, and API security. [IdentityServer documentation](https://identityserver4.readthedocs.io/en/latest/) is easy to follow and walks through several concrete examples of various OAuth flows.

IdentityServer is made to work with ASP.NET Core’s “middleware” concept, in which `Startup.cs` attaches IdentityServer behavior into the HTTP request pipeline hooks. API and MVC actions, for instance, can all use IdentityServer middleware for authorization, and all of your auth setup code appears together in `Startup`.

### **AVOID** making an identity system yourself
{: .text-red-300 }
