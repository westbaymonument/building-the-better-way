---
layout: default
title: IoC Containers
parent: Libraries
grand_parent: Development Guidelines
nav_order: 4
---

# IoC Containers
{:.no_toc}

1. TOC
{:toc}

### **DO** favor Microsoft’s IoC container, `Microsoft.Extensions.DependencyInjection`
{: .text-green-100 }
(MS Ext DI for short).

This is the IoC container first introduced in ASP.NET Core. It is being made available to other kinds of projects, such as long-running console applications in the 2019 timeframe. Whenever possible, start with this IoC container as our container of choice. It is consistent with our IoC usage patterns, and tools like AutoMapper and MediatR offer convenient helper methods for integrating with it, each with one line of code.

### **CONSIDER** StructureMap when MS Ext DI is not an option
{: .text-yellow-300 }
When working on a project that cannot use this preferred container, our trusted fallback is [StructureMap](http://structuremap.github.io/). This is a fully featured and stable library consistent with our IoC usage patterns, and we have used it effectively on many projects. With the advent of MS Ext DI, though, StructureMap’s maintainer has declared the library finished.

[Lamar](https://jasperfx.github.io/lamar/) is the spiritual successor to StructureMap and is meant to be a drop-in replacement. If you’re starting a new project that cannot use MS Ext DI, consider starting with Lamar and falling back to StructureMap as needed.

### **DO** select object lifetimes purposely
{: .text-green-100 }
MS Ext DI lets you configure the lifetime of objects created by the container. All IoC containers offer similar concepts, though they may use different names.

Use _Scoped_ lifetime for objects that should be constructed once per web request. Most types that we register with the IoC container naturally fall into this lifetime.

Use _Singleton_ lifetime for objects that should be constructed once and shared. Take care: such an object will surely be accessed from multiple simultaneous threads over countless web requests.

Use _Transient_ lifetime for objects that should be constructed anew every time they are requested from the IoC container. This is rare in practice, as Scoped is usually a more practical default in a web application.

### **CONSIDER** profiling memory usage of your application to confirm expired objects are disposed
{: .text-yellow-300 }
MS Ext DI holds transient objects created at root scope for the life of an application. If objects are created as transient but at a global level, then MS Ext DI will not dispose of these objects until the application is closed. Profiling memory usage can reveal if the root _disposables collection contains an increasing number of transient objects that are not being disposed. [MassTransit's EntityFrameworkSagaRepository was changed to use a scoped delegate DbContextFactory for this very reason](https://github.com/MassTransit/MassTransit/pull/1292/commits/a7280a926e8d031b78118ced037e531ac85d4e50).

### **DO** favor built-in type registration helpers
{: .text-green-100 }
Many types commonly-registered with the IoC container come with a natural recommended lifetime. For these types, the framework offers convenience methods for registering the type with the container. To minimize reinventing the wheel and to benefit from hard-learned lifetime recommendations, favor using these helper methods when possible.

For example, it is typical to use an EF DbContext with lifetime set to _Scoped_, meaning each web request gets a dedicated instance. Any action filter or MediatR handler which injects the `DbContext` gets the single `DbContext` dedicated to that web request. This is the most natural default lifetime for `DbContext`s, so we don’t even need to specify the lifetime when registering our `DbContext` type with the container:

```csharp
//Startup.cs
services.AddDbContext<DirectoryContext>(options =>
{
    options.UseSqlServer(connectionString);
});
```

### **DO** favor library-provided type registration helpers
{: .text-green-100 }
Many open source libraries offer one-line helper methods for registering their types with the IoC container, allowing the library maintainers to codify their recommendation for object lifetimes, assembly scanning, and the like.

For example, we can configure MediatR and AutoMapper in two lines, instead of explicitly scanning assemblies and registering types/lifetimes ourselves:

```csharp
//Startup.cs
var assembly = Assembly.GetExecutingAssembly();
services.AddMediatR(assembly);
services.AddAutoMapper(assembly);
```

### **AVOID** using the service locator pattern
{: .text-red-300 }
An application with effective IoC integration should rarely, if ever, have to call `IServiceProvider.GetService`(Type) directly. Instead, we favor simple constructor injection.

We only reach for explicit calls to `GetService()`  (or its equivalent in another IoC library) when we need to resolve a type where there is not a more natural constructor injection opportunity, such as in action filters in older versions of ASP.NET MVC.