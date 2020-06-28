---
layout: default
title: MediatR
parent: Libraries
grand_parent: Development Guidelines
nav_order: 2
---

# MediatR
{:.no_toc}

1. TOC
{:toc}

## Recommendations
{:.no_toc}

### **DO** use MediatR to organize your application into feature slices
{: .text-green-100 }
A typical ASP.NET MVC tutorial leads us in the wrong direction in one of two ways:

First, tutorials often place all of the feature’s implementation within controller actions, leading to complex and ever-growing controller classes that are essentially untestable. Automated tests of controller classes are difficult and unrealistic. Their natural reliance on `HttpContext` makes them both hard to construct and execute from within a test, and the amount of mocking necessary to “accomplish” that in a test leaves you with a test that has little or nothing to do with the behavior of the controller in a running web server.

Second, tutorials might hide some of these problems by extracting database work to a separate database _layer_ via helper classes as with the [Repository](https://docs.microsoft.com/en-us/dotnet/standard/microservices-architecture/microservice-ddd-cqrs-patterns/infrastructure-persistence-layer-design) pattern. We discourage these patterns, as they simply move the maintenance problem to another file: your repository classes are complex and ever-growing as features are added to the system.

There is little reason for code files that implement major sections of _unrelated_ features. Simply touching the same SQL tables does not make two features related enough to place their implementations together.

Instead, we use MediatR to organize the application into _features slices_ instead of layers. Each feature is independently responsible for its entire stack, from persistence to presentation and back. MediatR lets us do this while keeping our controllers very slim. We write tests for the MediatR handlers instead of our controllers.

### **DO** configure MediatR using `AddMediatR()` in ASP.NET Core Applications
{: .text-green-100 }
Outside of an ASP.NET Core application, follow MediatR’s documentation for wiring up MediatR types to your IoC container.

Doing that setup manually, however, is more work than necessary in an ASP.NET Core application. In this case, take advantage of an auxiliary NuGet package to do all of the assembly scanning and IoC type registering we need.

First, reference both the MediaR and `MediatR.Extensions.Microsoft.DependencyInjection` packages:

```xml
//*.csproj
<PackageReference Include="MediatR" Version="7.0.0" />
<PackageReference Include="MediatR.Extensions.Microsoft.DependencyInjection" Version="7.0.0" />
```

Next, configure MediatR for your application in a single line within `Startup.ConfigureServices`:

```csharp
//Startup.cs
services.AddMediatR(typeof(Startup).Assembly);
```

### **DO** Inject `IMediator` at the controller level
{: .text-green-100 }
MediatR enables us to leverage IoC through our entire application, starting at the “top” in our controller classes. We don’t construct handlers ourselves, nor do we inject `IHandler<T>`. Instead, we need only inject `IMediatR`, and use it to send incoming messages to their handlers. A typical controller constructor becomes trivial:

```csharp
//EmployeeController.cs
public class EmployeeController : Controller
{
    private readonly IMediator _mediator;

    public EmployeeController(IMediator mediator)
    {
        _mediator = mediator;
    }
    ...
```

### **DO** Use MediatR command / query types as your controller action argument
{: .text-green-100 }
Although a controller action might construct a MediatR command/query type, populate its properties, and then `Send()` it to its handler, we can often use the command/query type as the controller action argument itself. This makes our controller actions especially trivial, as it lets MVC’s built-in model binding naturally populate our message for us:

```csharp
//EmployeeController.cs
public async Task<ActionResult> Index(EmployeeIndex.Query query)
{
    var model = await _mediator.Send(query);
 
    return View(model);
}
 
...
 
[HttpPost]
[RequirePermission(RegisterEmployees)]
public async Task<ActionResult> Register(RegisterEmployee.Command command)
{
    if (ModelState.IsValid)
    {
        await _mediator.Send(command);
        return RedirectToAction("Index");
    }
 
    return View(command);
}
```

### **CONSIDER** Using MediatR command / query types as your view model
{: .text-yellow-300 }
Here, we declare a MediatR command type that doubles as a view model:

```csharp
//RegisterEmployee.cs
public class RegisterEmployee
{
    public class Command : IRequest<Response>
    {
        public string Email { get; set; }
 
        [Display(Name = "Initial Password")]
        [DataType(DataType.Password)]
        public string Password { get; set; }
 
        [Display(Name = "Confirm Initial Password")]
        [DataType(DataType.Password)]
        public string ConfirmPassword { get; set; }
 
        [Display(Name = "First Name")]
        public string FirstName { get; set; }
 
        [Display(Name = "Last Name")]
        public string LastName { get; set; }
 
        public string Title { get; set; }
 
        public Office? Office { get; set; }
 
        [Display(Name = "Phone Number")]
        public string PhoneNumber { get; set; }
    }
 
   ...
}
```

In our View,

```csharp
//Register.cshtml
@model EmployeeDirectory.Features.Employee.RegisterEmployee.Command
```

You can think of the form being submitted as also _being_ the command to execute. This makes controller actions trivial:

```csharp
//EmployeeController.cs
[HttpPost]
public async Task<ActionResult> Register(RegisterEmployee.Command command)
{
    if (ModelState.IsValid)
    {
        await _mediator.Send(command);
        return RedirectToAction("Index");
    }
 
    return View(command);
}
```

Not all view models will be 1:1 with MediatR command types. For instance, there may be views that need additional auxiliary data to render UI elements, where that data is not submitted back along with the form.

### **CONSIDER** defining your feature “slice” in a feature-defining wrapper class
{: .text-yellow-300 }
A typical feature slice in a web application will be made up of potentially-many classes: view models, command/query message types, handlers, message validators… Because these classes are meant to be developed and understood as a small and cohesive unit, we sometimes organize them into a single “feature file”, breaking from the typical class-per-file pattern.

When we opt for this structure, we wrap the feature’s classes in a surrounding do-nothing class named after the feature. For example, in the Headstart EmployeeDirectory sample application, we have a [DeleteEmployee feature file and wrapper class](https://bitbucket.org/headspring/headstart-employee-directory/src/26fd344158cf3daf20ab4d44a6b2ff60f2dd8048/src/EmployeeDirectory/Features/Employee/DeleteEmployee.cs)(_internal link_):

```csharp
//DeleteEmployee.cs
namespace EmployeeDirectory.Features.Employee
{
    using System;
    using System.Linq;
    using System.Threading;
    using System.Threading.Tasks;
    using FluentValidation;
    using Infrastructure;
    using MediatR;
    using Model;
    using Security;
 
    public class DeleteEmployee
    {
        public class Command : IRequest
        {
            public Guid Id { get; set; }
            public string FirstName { get; set; }
            public string LastName { get; set; }
        }
 
        public class Validator : AbstractValidator<Command>
        {
            private readonly IMediator _mediator;
 
            public Validator(IMediator mediator, UserContext context)
            {
                _mediator = mediator;
                RuleFor(x => x.Id)
                    .NotEqual(context.User.Id)
                    .WithMessage("Employees cannot delete themselves.");
 
                RuleFor(x => x)
                    .MustAsync(NotHaveManageSecurityPermission)
                    .WithMessage(
                        "You cannot delete an employee who has permission to " +
                        "manage security. Please coordinate with your system " +
                        "administrators first.");
            }
 
            private async Task<bool> NotHaveManageSecurityPermission(Command command, CancellationToken token)
            {
                var permissionsForEmployeeToDelete =
                    await _mediator.Send(new Permissions.Query { EmployeeId = command.Id }, token);
 
                return !permissionsForEmployeeToDelete.Contains(Permission.ManageSecurity);
            }
        }
 
        public class CommandHandler : RequestHandler<Command>
        {
            private readonly DirectoryContext _database;
 
            public CommandHandler(DirectoryContext database)
            {
                _database = database;
            }
 
            protected override void HandleCore(Command message)
            {
                var employee = _database.Employee.Find(message.Id);
 
                var roleAssignments =
                    _database.EmployeeRole
                        .Where(x => x.Employee.Id == message.Id)
                        .ToArray();
 
                foreach (var roleAssignment in roleAssignments)
                    _database.EmployeeRole.Remove(roleAssignment);
 
                _database.Employee.Remove(employee);
            }
        }
    }
}
```

In this example, we see a few benefits:

- A developer can understand the Delete Employee feature at a glance by simply reading this small file top to bottom. The inner classes are in the same order they would be used as the user requests a deletion.
- The inner classes’ names become trivial: Command, Validator, CommandHandler. We don’t have to define many long-named classes that all share a common prefix, as the wrapper class accomplishes that for us.
- By using a wrapper class instead of a namespace, we can still refer to these types from elsewhere clearly, without VS/ReSharper suggesting that the name prefixes be confusingly removed:
    - public async Task<ActionResult> Index(EmployeeIndex.Query query)

### **DO** test handlers by calling `Send()`
{: .text-green-100 }
One might assume that the best way to write automated tests for a handler would be to construct it, pass in dependencies, and then invoke `Handle()`, asserting on the returned result and any side effects. However, doing so tests the handler in isolation while missing important opportunities to test the larger integration with the application. Is the IoC set up correctly? Will the handler work correctly in the context of the surrounding database transaction behavior? Have we correctly defined only one handler for this message type?

We prefer invoking handlers in our tests exactly as we do from a controller: by sending a message through an IoC-resolved IMediator, using as much of the production IoC configuration as possible.

This technique is discussed at length in [Testing Standards](development-guidelines/testing/#testing-guidelines).

### **AVOID** Send()-ing another message from within a handler
{: .text-red-300 }
Although tempting, we discourage sending a message from within a handler. Once an application begins to take advantage of MediatR Behaviors for cross-cutting concerns such as wrapping each `Send()` in a transaction, a `Send()` from within a handler may be provably incorrect as it would enlist in the same behavior pipeline as the initial/surrounding `Send()` in progress. To avoid this type of bug, and to enable opting into Behaviors at any time in the life of an application, simply avoid calling `Send()` from within a handler as a matter of course.