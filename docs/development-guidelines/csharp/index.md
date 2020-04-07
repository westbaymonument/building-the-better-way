---
layout: default
title: C#
nav_order: 4
parent: Development Guidelines
has_children: false
---

# C#
{:.no_toc}

This guide establishes a default approach to development in C# at Headspring that defines our standards and sets clear expectations for Engineers. It also serves as reference material to aid in streamlining new Engineer onboarding and helps everyone become more self-sufficient.

Code that has a predictable structure and follows guidelines enables developers to adjust to the purpose of code and not an unfamiliar, or distracting, convention. Established conventions promote readability and affirm expectations that developers will have on first inspection. Code becomes easier to navigate and simpler to understand when we follow established conventions and standards. Coding standards take into account that other developers will read and maintain the code in the future.

- TOC
{:toc}

## Importance of Consistency

Many of our projects are not "greenfield." We often take over existing, mature code bases or work alongside existing client engineering teams or even other consultant firms. It's tempting to forge ahead with your own conventions, naming styles, etc. (many of which exist in this document) without considering the impact on the overall code base. However, the correct approach is to adjust your style to match what's already established.

The code we write will be read by many other people for years to come, starting with our fellow developers and tech leads. Once we write software for a client, we hand it off and then leave it with them, with the goal that either they or any other team of developers could easily maintain it. If the code base is a mixed bag of style and structure, readers will find it difficult to parse, which makes it difficult to maintain. The goal is to have a code base that looks like it was written by one developer.

### **DO** default on new projects to conform to this guide
{: .text-green-100 }
### **DO** conform to pre-existing code styles in a project, even when they differ from this guide
{: .text-green-100 }
### **AVOID** mixing multiple coding styles in a single code base
{: .text-red-300 }
### **DO** utilize tooling configuration files to control style and structure automatically across developer environments
{: .text-green-100 }
Visual Studio uses `.editorconfig` files and R# uses `*.DotSettings` files to persist configurations that can be shared across environments and stored in source control

Headspring’s default ReSharper settings can be found in the following repository:

[Resharper Configuration](https://bitbucket.org/headspring/resharper-configuration/src/master/)(_internal repository_)

## General C# Standards

### **DO** use var instead of explicit types where possible.
{: .text-green-100 }
Exception: Needing an explicit sub-type, such as:

```csharp
ReadOnlyCollection<People> people = GetPeople(); // returns List<People>()
```

### **DO NOT** use an explicit this qualifier when referencing member fields and methods
{: .text-red-300 }
```csharp
// Bad
public void SampleMethod(string name)
{
    this.Name = name;
    this.AddToList(name);
}

// Good
public void SampleMethod(string name)
{
    Name = name;
    AddToList(name);
}
```

### **DO** use aliases for built-in types instead of BCL class names
{: .text-green-100 }
```csharp
// Bad
String.IsNullOrEmpty(person.Name);

// Good
string.IsNullOrEmpty(person.Name);
```

### **CONSIDER** using string interpolation instead of string.Format()
{: .text-yellow-300 }
```csharp
// Bad
var message = string.Format("You live in {0}, {1}.", address.City, address.State);

// Good
var message = $"You live in {address.City}, {address.State}.";
```

### **DO** use four spaces instead of two spaces or tabs per indentation level
{: .text-green-100 }
```csharp
// Bad
public void DoSomething()
{
••Foo();
}

// Good
public void DoSomething()
{
••••Foo();
}
```

### **DO NOT** have more than one sequential empty (whitespace) line
{: .text-red-300 }
```csharp
// Bad
public void DoSomething()
{
   Foo();
   Bar();


   Baz();
}

// Good
public void DoSomething()
{
   Foo();
   Bar();

   Baz();
}
```

### **DO** remove unused or redundant using statements (or directives)
{: .text-green-100 }
### **DO** place braces for multi-line statements on their own line
{: .text-green-100 }
```csharp
// Bad
public void DoSomething() {
   Foo();
}

// Good
public void DoSomething()
{
   Foo();
}
```

### **DO NOT** follow or precede opening or ending braces with a blank line
{: .text-red-300 }
```csharp
// Bad
public void DoSomething()
{

   Foo();
   Bar();

}

// Good
public void DoSomething()
{
   Foo();
   Bar();
}
```

## Basic Layout Of Class Members

Code should be readable so developers with no background on a project can navigate a class without having to adjust to a unique convention. Ordering the members of a class provides an expected structure that allows developers to understand new code quickly and thoroughly.

The statements below apply to a green-field project - where we get to make decisions on new classes that we set up. Often times, we are called upon to help with an existing system. In those cases, we should follow the established patterns of the client.

### **DO** write class members in the conventional order below, so that members are found more easily
{: .text-green-100 }
**Order of Members**

Place members for all classes, interfaces and structs in this order:

Constant Fields
Fields
Constructors
Finalizers (Destructors)
Delegates
Events
Enums
Interfaces (interface implementations)
Properties
Indexers
Methods
Structs
Classes

For each member type above, order by access modifier as follows:

public
internal
protected internal
protected
private

Within the access modifiers, place static elements before instance (non-static) elements.

Place readonly elements before mutable elements.

## Naming Conventions

Naming conventions enhance readability and describe domain objects throughout the system. At a glance, a developer should be able to determine the purpose of types, methods, etc. without having to look for context in the implementation.

### **DO** use PascalCasing for class names, method names, properties and namespaces
{: .text-green-100 }
### **DO** use camelCasing for method arguments, local variables and field names
{: .text-green-100 }
Exceptions to casing should include special business terminology used in domain objects. ReSharper might complain that the abbreviations have violated casing conventions. Add  abbreviations to the ignore list in the ReSharper configuration file to skip analysis of domain abbreviations. Share DotSettings files at the team level.

### **DO NOT** Use Hungarian Notation, prefixing data type abbreviations to variable or field names
{: .text-red-300 }
### **DO** use capitalization to separate two different words in the name of an identifier
{: .text-green-100 }
### **AVOID** Using abbreviations or shorter versions of words - as in GetWin for GetWindow
{: .text-red-300 }
Exception: Abbreviations commonly used as names, such as Id, Xml, Ftp, Uri, etc.

### **DO** use PascalCasing for abbreviations of three characters or more
{: .text-green-100 }
### **DO NOT** use underscores to separate words
{: .text-red-300 }
Exception: Test method names are often written with underscores between words. In this specific instance, you are using underscores to increase readability of verbose and descriptive method names that would be hard to visually parse otherwise.

### **CONSIDER** creating a list of exclusions in a shared ReSharper dotsettings file to hold well-known domain abbreviations
{: .text-yellow-300 }
Example:

MASMModule where MASM stands for Medical Administrative Service Module

MROWeb where MRO stands for Medical Review Officer

### **DO** use noun or noun phrases to name a class
{: .text-green-100 }
### **AVOID** using overly generic type names
{: .text-red-300 }
A good name should describe everything a class or routine does. Some common suffixes to avoid are:

Manager
Builder
Writer
Getter
Setter
Provider
Facade

### **DO** prefix interfaces with the letter 'I'
{: .text-green-100 }
Interface names should be nouns, noun phrases or adjectives.

## Visibility of Types and Members

Favor more limiting over less limiting access modifiers. Use private for methods that are not to be accessed outside the current class, and public for methods that represent publishing an accessible interface and are not restricted. Limit access modifiers to prevent implementation details from leaking outside of the current context.

### **AVOID** using internal or protected modifiers
{: .text-red-300 }
`Internal` modifiers are not typically useful unless you have a special use case, such as library development. `Protected` modifiers may be an indicator that class inheritance is being abused.

### **DO** use explicit modifiers, even when it matches the default access level
{: .text-green-100 }
Example: Field declarations are `private` by default, but adding an explicit private modifier clearly indicates that intent.

### **DO** use private modifier on types or members that should only be accessed inside the class, and public for all others
{: .text-green-100 }
When considering how to access types and their methods and properties, consider how testable the class is. If you over-limit your class with private modifiers, you make it difficult or impossible to write tests for all of its functionality.

## DRY Principle

Organizing your code in smaller, maintainable, and reusable chunks makes the code stronger and less prone to errors, because each chunk of code is testable and performs its function as designed.  Repeating code in multiple places is more prone to error and wastes time because you'll have to modify the implementation in multiple places.

However, the DRY principle can also be overused. Overly “dry” code can be difficult to read, with too many logical calls and redirects to follow in order to understand what it actually does.

Additionally, when we write applications using feature folders & MediatR, we might find ourselves writing the same query or update operation in a few different handlers. It's tempting to refactor that repetition out into some sort of utility or service, but first you need to determine if the repetition is a result of coincidence or intent. Command and Query handlers, for example, each represent a specific piece of business logic. Duplicate code across multiple handlers is more likely coincidental rather than intentional, and not a good candidate for refactoring into utility methods.

An approach you can consider is WET - “Write everything twice.” Code duplicated twice is usually OK, as it saves us from the effort of premature optimization based on gut reactions. Instead we look at patterns that emerge when code is duplicated three or more times. This allows us to examine the actual use case for the code and make intelligent decisions about how to properly refactor.

Additional Information: https://dev.to/wuz/stop-trying-to-be-so-dry-instead-write-everything-twice-wet-5g33

### **CONSIDER** refactoring code into a common reusable method when that code has been duplicated three or more times
{: .text-yellow-300 }
### **AVOID** overusing the DRY principle to purposefully duplicated code
{: .text-red-300 }
Code should be purposeful and exist to resolve a very specific problem. If refactoring seemingly duplicated code changes the characteristics of code, or the functionality, then leave it alone.

## Use of Regions

Regions are an IDE feature from the .NET Framework 1.x era (c. 2002) that was used to hide generated designer code before 2.0 introduced partial classes. Code readability can be greatly hindered when your IDE hides details. If your source code file becomes too long then consider refactoring your implementation rather than hiding code in regions.

### **DO NOT** use regions
{: .text-red-300 }
## Self-Documenting Code

Could should express intent through mindful naming and recognizable patterns. Self-describing code alleviates the need for forced or verbose commenting. Code comments should only be used to document implementation choices that are not self-explanatory such as terse language features like RegEx patterns or bit masks. If you think you need to write a comment, are you describing “why it is” or “what it is”? If you find the need to write a “what it is” comment, then you should reconsider the naming and structure of your code to better convey intent.

Additional sources:

[Clean Code](https://www.amazon.com/Clean-Code-Handbook-Software-Craftsmanship-dp-0132350882/dp/0132350882/ref=mt_paperback?_encoding=UTF8&me=&qid=) (Martin) chapters 2 (Meaningful Names) and 4 (Comments)

Headspring “Mindful Development - Week 4” video (~20:00 - 27:00 segment)(_internal_)

### **DO** use mindful naming and recognizable patterns when naming methods, variables, etc. to express intent
{: .text-green-100 }
Parameter names, for example, should be descriptive enough that the name and its type can be used to determine its use in most scenarios.

### **CONSIDER** providing any additional information associated with your code in the commit text, rather than a comment block
{: .text-yellow-300 }
Comment blocks grow stale, whereas the commit message lives with the history of the change.

### **DO NOT** push TODO comments to main or dev branches
{: .text-red-300 }
`TODO` comments can be useful for a developer to track in-flight work, but they should never persist outside of the feature or bug branch. `throw new NotImplementedException();` near the source of the `TODO` to ensure that the work is completed before merging the branch.

For longer-lived “todo”s, track future work in JIRA, or the relevent project management tool, so that it can remain visible across the team, be planned, and tracked.

### **AVOID** the use of comments to explain poorly written code
{: .text-red-300 }
If you find yourself needing to write a comment to explain what you have done, that may be an indicator that some refactoring could be done to make the code more readable.

### **DO** use comments when documentation is necessary or required by the client
{: .text-green-100 }
XML reference documentation, for example, can be utilized by third-party tools such as Swagger to document a RESTful API.

### **DO NOT** comment out code
{: .text-red-300 }
Source control provides a history of code changes if a change needs to be reverted or referred back to.

## Boy Scout Rule

In any project over time the quality of the code base will tend to degrade and technical debt will accrue. In an effort to proactively combat the accrual of technical debt, we make a conscientious effort to leave things better than we found it. If you're making changes to a method, for example, take a few minutes and see if there is anything that can be improved in a short amount of time. The change doesn't have to be big - just better. If you continue to make these little changes, then over time the overall quality of the code will dramatically improve.

Read Source Control Guidance - Make Tool Changes On Own Commit for details on how to capture refactor work in source control.

Additional sources:

[Clean Code](https://www.amazon.com/Clean-Code-Handbook-Software-Craftsmanship-dp-0132350882/dp/0132350882/ref=mt_paperback?_encoding=UTF8&me=&qid=) (Martin) chapters 1 (The Boy Scout Rule)

### **DO** keep your changes in the scope of the feature or bug you are working on
{: .text-green-100 }
Tech Leads need to be able to review your changes, and refactoring an unrelated class or method will make it more difficult and time consuming to determine the cause and impact of the change.

### **CONSIDER** renaming variables, parameters, and methods
{: .text-yellow-300 }
For example, variable names that are named ambiguously or incorrectly and require investigation to determine their purpose, such as day vs. birthDay, or price vs. priceAfterTax.

### **CONSIDER** fixing small bugs you discover
{: .text-yellow-300 }
Bugs discovered in scope of your feature that can be fixed quickly should be fixed. However, if you discover a larger bug, or bugs that are not in your feature’s scope, create a bug ticket instead.

### **CONSIDER** refactoring large classes to increase readability
{: .text-yellow-300 }
Large classes are often an indication of an over-generalized utility class.

### **CONSIDER** potential merge or team conflicts for your changes
{: .text-yellow-300 }
Refactoring code that other teams might be working on could cause cumbersome merge conflicts. Ensure your changes do not upset or delay overall progress.

### **CONSIDER** creating “Refactor” or “Cleanup” tickets to track larger cleanup efforts you identify
{: .text-yellow-300 }
These changes should be coordinated with the team and project leadership to ensure that they are not disruptive.

## Composition vs Inheritance

Inheritance is an OOP pattern that can easily be abused.

Misuse of inheritance can lead to:

- Tight coupling between two concrete classes
- Fragile base classes
- Weakened encapsulation
- Issues or Complications with Testing
- Additional Maintenance Overhead

### **DO** favor composition over inheritance
{: .text-green-100 }
Composition gives our design higher flexibility, with the ability to modify behavior in the future without violating contracts.

### **DO** use interfaces to define contracts between classes
{: .text-green-100 }
If you define interfaces at key points of your application, you give careful thought to the behavior they should support and commit to that behavior.

### **DO NOT** use inheritance when the subclass is not a proper sub-type of the superclass
{: .text-red-300 }
Classes that use a lot of `virtual`, `override` and `base` keywords are hard to read. Determining the purpose of a class that pulls or changes behavior from a base class (or a base of a base class) makes it difficult to track the actual behavior being performed. Subclasses should only be used to add to the functionality of a base class, not modify it.

### **DO NOT** inherit across domain boundaries
{: .text-red-300 }
If common mechanical structure is desired across domains, use a common utility base class instead.

### **CONSIDER** single inheritance for establishing conventions in a single layer of an application
{: .text-yellow-300 }
For example, we often create a base controller class that contains conventions we want available to all other controllers, such as a helper method for returning specifically formatted JSON responses.

### **AVOID** inheritance of more than three levels
{: .text-red-300 }
Inheritance of more than three levels is a strong indicator that you should consider refactoring to composition.

An example of acceptable three level inheritance is when you need to utilize a third party class before extending it with your own base implementation.

```csharp
public class MyBaseController : ClientLibrary.ClientBaseController
{
   // …
}

public class EmployeeController : MyBaseController
{
   // …
}
```
