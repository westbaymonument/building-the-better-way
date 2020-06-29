---
layout: default
title: Automapper
parent: Libraries
grand_parent: Development Guidelines
nav_order: 1
---

# Automapper
{:.no_toc}

Our typical application architecture follows a CQRS structure. When designing our command or query responses we do not want to return domain objects directly. This type of coupling limits our flexibility and adds maintenance overhead that is otherwise undesirable. Furthermore, rarely do we want the full domain object being returned to the view, as this can expose privacy or security concerns.

Instead, we follow a few basic rules:

- Each model will be designed for one and only one view or api response.
- Only the information needed to render or model bind is contained in the model.

In most cases, these models are merely subsets of data from the larger domain model. Writing the mapping code for these is tedious. AutoMapper is a tool we use to handle a lot of the boring, repetitive property assignment code that happens around mapping Domain objects to view and api models (MediatR response classes).

The power and convenience of AutoMapper, however, lends itself to overuse and abuse. AutoMapper was created to help enforce our conventions around naming and around dedicated view models per view. AutoMapper misuses often coincide with forgetting that goal.

1. TOC
{:toc}

## Recommendations

### **DO** initialize AutoMapper once with `Mapper.Initialize` at AppDomain startup in legacy ASP.NET
{: .text-green-100 }
AutoMapper's static initialization is designed to build configuration once, and cache it.

### **DO** use the [AutoMapper.Extensions.Microsoft.DependencyInjection](https://www.nuget.org/packages/automapper.extensions.microsoft.dependencyinjection/) package in ASP.NET Core with services.`AddAutoMapper(assembly[])`
{: .text-green-100 }
The extensions package will perform all the scanning and dependency injection registration. You only need to declare `Profile` configuration.

### **DO NOT** call `CreateMap` on each request
{: .text-red-300 }
This is difficult to do now, but do not create configuration for each mapping request. Mapping configuration should be done once at startup.

### **DO NOT** use [inline maps](https://docs.automapper.org/en/stable/Inline-Mapping.html)
{: .text-red-300 }
Inline maps are good for very simple scenarios, but you lose the ease of configuration validation.

### **DO** organize configuration into [profiles](https://docs.automapper.org/en/stable/Configuration.html#profile-instances)
{: .text-green-100 }
Profiles allow you to group common configuration and organize mappings by usage. This lets you put mapping configuration closer to where it's used, instead of a single file of configuration that becomes impossible to edit/maintain.

### **CONSIDER** organizing profile classes close to the destination types they configure
{: .text-yellow-300 }
Especially when using a vertical slice architecture, mapping configuration is easier to maintain when it's close to the destination type it configures.

### **DO NOT** access the static Mapper class inside a profile
{: .text-red-300 }
If you need to get access to mapping configuration or a mapper object, all mapping methods contain overloads and all mapping extensions take a `ResolutionContext` object, which includes a contextual `Mapper`. This `Mapper` will be contain any cached objects, services, and will be scoped appropriately.

### **DO NOT** use a DI container to register all profiles
{: .text-red-300 }
AutoMapper already has configuration methods to add all profiles from assembles (`AddProfiles`) and an extension to `IServiceCollection` to add all profiles (`services.AddAutoMapper`).

### **DO NOT** inject dependencies into profiles
{: .text-red-300 }
Profiles are static configuration, and injecting dependencies into them can cause unknown behavior at runtime. If you need to use a dependency, resolve it as part of your mapping operation. You can also have your extension classes (resolvers, type converters, etc.) take dependencies directly.

### **CONSIDER** using [configuration options supported by LINQ](https://docs.automapper.org/en/stable/Queryable-Extensions.html#supported-mapping-options) over options not supported by LINQ
{: .text-yellow-300 }
LINQ query extensions have the best performance of any mapping strategy, so it's better to use it as much as you can.

### **AVOID** [before/after map configuration](https://docs.automapper.org/en/stable/Before-and-after-map-actions.html)
{: .text-red-300 }
If you have to do complex mapping behavior, it might be better to avoid using AutoMapper for that scenario.

### **AVOID** [ReverseMap](https://docs.automapper.org/en/stable/Reverse-Mapping-and-Unflattening.html) in cases except when mapping only top-level, non-flattened properties
{: .text-red-300 }
Reverse mapping can get very complicated very quickly, and unless it's very simple, you can have business logic showing up in mapping configuration.

### **DO NOT** put any logic that is not strictly mapping behavior into configuration
{: .text-red-300 }
AutoMapper should not perform any business logic, only mapping configuration.

### **DO NOT** use `MapFrom` when the destination member can already be auto-mapped
{: .text-red-300 }
For example, this is not necessary:

```csharp
CreateMap<Foo, FooDto>()
    .ForMember(dest => dest.Name, opt => opt.MapFrom(src => src.Name))
    .ForMember(dest => dest.OrderTotal, opt => opt.MapFrom(src => src.Order.Total));
```

AutoMapper automaps by name, and flattens properties, so there is no need to explicitly map names that already match. Doing so results in more code than mapping manually, making the use of AutoMapper pointless.

### **DO NOT** use AutoMapper except in cases where the destination type is a flattened subset of properties of the source type
{: .text-red-300 }
AutoMapper is designed for projecting a complex model into a simple one. It can be configured to map complex scenarios, but this results in more confusing code than just assigning properties directly. If your configuration is complex, don't use this tool.

### **DO NOT** use AutoMapper to support a complex layered architecture
{: .text-red-300 }
Please don't. Use [vertical slices](https://www.youtube.com/watch?v=SUiWfhAhgQw) instead.

### **AVOID** using AutoMapper when you have a significant percentage of custom configuration in the form of `Ignore` or `MapFrom`
{: .text-red-300 }
The "Auto" is for "automatic" and if it's not "Auto" then don't use this library, it will make things more, not less complicated.

## Modeling

### **DO** flatten DTOs
{: .text-green-100 }
AutoMapper can handle mapping properties `Foo.Bar.Baz` into `FooBarBaz`. By flattening your model, you create a more simplified object for consumers that won't require a lot of navigation to get at data.

### **AVOID** sharing DTOs across multiple maps
{: .text-red-300 }
It can get confusing if you need to change a DTO and you accidentally affect another request. Model your DTOs around individual requests, and if you need to change it, you only affect that one request.

### **DO** create inner types in DTOs for member types that cannot be flattened
{: .text-green-100 }
For example:

```csharp
public class OrderDto
{
    public List<OrderLineItemDto> LineItems { get; set; }
    public class OrderLineItemDto
    {
    }
}
```

This ensures that your DTOs aren't accidentally shared with other requests, resulting in undesired coupling.

### **DO NOT** create DTOs with circular associations
{: .text-red-300 }
AutoMapper does support it, but it's confusing and can result in quite bad performance. Instead, create DTOs for each level of a hierarchy you want.

### **AVOID** changing DTO member names to control serialization
{: .text-red-300 }
Serialization frameworks already have configuration to affect serialization, such as attributes. If member names are different, you'll need to explicitly configure member names, making the "Auto" in AutoMapper gone.

### **DO** put common simple computed properties into the source model
{: .text-green-100 }
AutoMapper supports mapping from methods if the names match or are prefixed with "Get". For example, `GetFullName()` will map to `FullName`.

### **DO** put computed properties specific to a destination model into the destination model
{: .text-green-100 }
Don't put a computed property on the source model if it's specific to the destination type. This will muddy the responsibility of the source model.

## Execution

### **CONSIDER** using [query projection](https://docs.automapper.org/en/stable/Queryable-Extensions.html) (`ProjectTo`) over in-memory mapping
{: .text-yellow-300 }
Projections are much more performant than in-memory mapping, as LINQ query providers will use the Select expression to generate the exact SQL needed to populate your DTO directly.

### **DO NOT** abstract or encapsulate mapping behind an interface
{: .text-red-300 }
It's a waste of time. Just use `IMapper` directly.

### **DO** use mapping options for runtime-resolved values in projections
{: .text-green-100 }
This allows you to have parameterized projections, making sure you don't over-fetch data and filter on the client.

### **DO** use mapping options for resolving contextualized services in in-memory mapping
{: .text-green-100 }
You can use `ResolutionContext.Mapper.ServiceCtor` or dependency injection directly on your resolvers, type converters, etc.