---
layout: default
title: Entity Framework
parent: Libraries
grand_parent: Development Guidelines
nav_order: 5
---

# Entity Framework
{:.no_toc}

1. TOC
{:toc}

### **DO** favor EF Core over EF for new applications
{: .text-green-100 }
The EF project began on .NET Framework. The EF Core project began when .NET Core arrived, and can run on either .NET Framework or .NET Core. During 2019, EF will begin working on .NET Core as well. With .NET Framework reaching end of life in 2020 in favor of .NET Core, under the new brand “.NET 5”, that raises the question of which of these two frameworks is actually meant to survive in the long run, and therefore which we should choose for new projects.

Microsoft provides the following guidance: EF6 is a supported product, but they are not investing in new feature development. The goal of porting EF6 to run on .NET Core is merely to provide an easier path for legacy applications to migrate to .NET Core and .NET 5. EF Core is the modern alternative and first choice for new apps.

EF Core requires less customization for our typical naming conventions, performs no risky automatic migrations, and is overall a simpler framework to learn.

### **DO** `Include()` related entities purposely
{: .text-green-100 }
When you query from a `DbSet`, you have the opportunity to state which related entities should be included in the initial round trip:

```csharp
var details = context.OrderDetail
    .Include(x => x.Order)
    .Where(x => x.Order.PurchaseTime >= DateTime.Today)
    .OrderBy(x => x.Order.PurchaseTime)
    .ToArray();
```

We must call `Include()` with deliberate intent. Here, the call to `Include()` says, “We have every intention of accessing `details[n].Order` soon after this statement, so eagerly include those related entities in the single round trip to the database. Otherwise, we would be relying on lazy loading, and those `details[i].Order` accesses would each be a new query to the database.

We need to carefully consider calls to `Include()` on each query, with purpose. If we call `Include()` when we will not actually use that data, we have wasted resources fetching potentially far more data than we needed, and we have made our generated `SELECT` statement needlessly complex, which may impact SQL Server’s optimization efforts. If we fail to call `Include()` when we would benefit from it, we silently run into the “N+1” problem, meaning that the system is wastefully making many separate round trips to the database, resulting in poor performance.

### **DO NOT** share entities across multiple `DbContext`s
{: .text-red-300 }
We often have a single `DbContext` in play for a single web request. However, in tests and in long-running background processes, we are more likely to have multiple short-lived `DbContext` instances. As soon as there are multiple `DbContext`s near each other, we can very easily fall into traps as a consequence of sharing entity instances across those contexts.

One rule protects us: Entities must only be used in the `DbContext` they were queried through.

When we violate this rule, we can for instance cause unintended behavior, such as duplicate `INSERT`s or silently failing to perform an intended `UPDATE`. The second `DbContext` doesn’t recognize instances you fetched via the first `DbContext`, and cannot track new changes that were being tracked by the first. For a complete example of the problem in action, see [AttemptedChangeTrackingAcrossDbContexts()](https://bitbucket.org/headspring/headstart/src/1ac68ab3c09d02148d886593eb3e05fdd0832b38/src/16.%20ORMs/5.%20LeakyAbstractionTests.cs#lines-73)(_internal repository_) in the HeadStart solution.

In long-running processes, limit the scope and lifetime of entities fetched from a `DbContext`, so that you avoid accidentally using them in the wrong `DbContext` later.

In tests, follow the guidance in [Testing Standards](http://localhost:4000/development-guidelines/testing/) to avoid the need to manually deal with DbContexts directly.

### **DO NOT** use long-lived `DbContext`s
{: .text-red-300 }
`DbContext` was designed with short-lived instances in mind. You construct one, perform a few related commands and queries against that instance, and dispose of it. During that short lifetime, it maintains an internal cache of fetched entities and tracks changes on those entities in order to support the `SaveChanges()` operation.

A long lived instance thwarts the intended use case, resulting in an object that grows larger and larger the more it is used. Your long-lived `DbContext` may seem to behave well in a development environment, and then perform poorly in a production environment.

Long-lived `DbContext`s also increase the chance of violating DO NOT share entities across multiple `DbContext`s.

### **CONSIDER** `DbContext` helper methods for opening and closing transactions for each unit-of-work
{: .text-yellow-300 }
We need to be careful with calling `SaveChanges` in the context of completing a database transaction. A typical web application may have a unit-of-work global action filter which either completes or rolls back the `DbContext`’s transaction at the end of a web request, depending on whether an exception has been thrown. However, the call to `SaveChanges()` itself may throw, after we’ve decided that the controller action hasn’t thrown. To correctly deal with these subtleties, we often include `BeginTransaction()` and `CloseTransaction()` helper methods on the `DbContext` itself.

See [these methods](https://bitbucket.org/headspring/headstart/src/1ac68ab3c09d02148d886593eb3e05fdd0832b38/src/16.%20ORMs/2.%20StoreContext.cs#lines-116:154)(_internal repository_) in the example `DbContext` subclass in the HeadStart solution.

### **CONSIDER** a global action filter for your unit-of-work
{: .text-yellow-300 }
It is often useful to align database transaction lifetime with web request lifetime, allowing each web request to be all-or-nothing.

Consider your use cases, though, as this is a balance between developer-facing simplicity and efficiency. Starting a transaction at the beginning of the request may force the database to hold on to locks longer than strictly necessary, while business logic is executing. Your system may involve different kinds of data stores (ie. SQL + Elasticsearch + MongoDb, where a fair number of actions simply won’t need an implicit SQL transaction).

```csharp
//Startup.cs
public void ConfigureServices(IServiceCollection services)
{
   services.AddMvc(options =>
   {
       ...
       options.Filters.Add<UnitOfWork>();
       ...
   });
```

```csharp
//UnitOfWork.cs
public class UnitOfWork : IActionFilter
{
    private readonly DirectoryContext _database;
 
    public UnitOfWork(DirectoryContext database)
        => _database = database;
 
    public void OnActionExecuting(ActionExecutingContext context)
        => _database.BeginTransaction();
 
    public void OnActionExecuted(ActionExecutedContext context)
        => _database.CloseTransaction(context.Exception);
}
```

### **CONSIDER** AutoMapper’s `ProjectTo()`
{: .text-yellow-300 }
See [Automapper Usage Guidelines](/development-guidelines/libraries/automapper.html#consider-using-query-projection-projectto-over-in-memory-mapping)

### **CONSIDER** `new {...}` Projection of Specific Columns
{: .text-yellow-300 }
When selecting whole entity types with EF, all columns from the corresponding table are selected. If you know you’re only going to use a subset of the relevant columns, consider using anonymous object syntax in your final `Select(...)` call, so that EF knows to trim down the actual SQL generated: `.Select(x => new { x.ColumnA, x.ColumnD })`.

### **AVOID** Many-to-many Mappings
{: .text-red-300 }
Many-to-many table relationships are valid, of course. When we use them in EF, though, avoid complex mapping declarations for navigating the many-to-many relationship. Instead, declare an entity corresponding with the many-to-many table, and use it explicitly. For examples, see [_Avoiding many-to-many mappings in orms_](https://lostechies.com/jimmybogard/2014/03/12/avoid-many-to-many-mappings-in-orms/) 

### **AVOID** Attribute-based mappings
{: .text-red-300 }

### **AVOID** Automatic Code First Migrations
{: .text-red-300 }
This applies to EF, and does not apply to EF Core.

When using EF, [Automatic Code First Migrations](https://docs.microsoft.com/en-us/ef/ef6/modeling/code-first/migrations/automatic) are enabled by default. This feature is extremely risky as it thwarts any effort we make to enforce traceable database change management with tools like RoundhousE. When setting up an EF project, the first step is to [disable this dangerous feature](https://stackoverflow.com/questions/14654055/how-can-i-disable-code-first-migrations).

### **AVOID** declaring excessive/unused bidirectional navigation properties
{: .text-red-300 }
The following example model shows several options for describing the relationship between two entities:

```csharp
//Model.cs
public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
    public List<Post> Posts { get; set; }
}
 
public class Post
{
    public int PostId { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }
 
    public int BlogId { get; set; }
    public Blog Blog { get; set; }
}
```

It is _not a requirement_ to have all of the following: a collection of Posts on Blog, a BlogId FK property on Post, and a Blog navigation property on Post. If these are all in fact used by your system, then include them. Any of these not in fact used by your system should be omitted until they would be useful. The pairing of BlogId and Blog on the Post class is especially confusing when code sets either one but not the other. Study the [Relationships Documentation](https://docs.microsoft.com/en-us/ef/core/modeling/relationships) to learn more about how EF Core infers table relationship information from your property declarations. Keep your models simple and use-case driven.

### **AVOID** unintended `SELECT *` queries
{: .text-red-300 }
There are two ways to easily perform a `SELECT *` query when you intended a more efficient query.

First, it is easy to perform a `SELECT *` by mistake when you confuse `IQueryable` with `IEnumerable`. EF queries work with `IQueryable<T>` types, which allow you to efficiently chain method calls to build up a query: `.Select(...).Where(...).OrderBy(...).ThenBy(...)`. These `IQueryable` method calls are similar in appearance to the `IEnumerable` extension methods of the same names. `IQueryable` methods do not perform queries, but instead build up a description of a `SELECT` statement. The statement is finally executed when it has to, such as when iterating with `foreach` or calling `ToList()`/`ToArray()`. A query that is “realized” with `ToList()`/`ToArray()` is now an `IEnumerable`, and all subsequent LINQ method calls will be in memory on the C# side.

Be deliberate in your code when you intend to actually execute a query vs. when you intend to modify an `IQueriable` prior to execution.

For a complete example of the problem in action, see [`]QueryableVsEnumerable()`](https://bitbucket.org/headspring/headstart/src/1bdb118daf023a9f5c92df2ed28f54c6c48a1b63/src/16.%20ORMs/5.%20LeakyAbstractionTests.cs#lines-254)(_internal repository_) in the HeadStart solution.

Second, it is easy to perform a `SELECT *` by mistake, even when using `IQueryable`, by including a lambda expression that cannot be trivially translated to SQL. Consider the lambda expressions passed to `IQueryable<T>` methods `Select()`, `Where()`, `OrderBy()`, … These are meant to become part of the SQL generated by EF. When the lambda expression cannot be trivially translated to SQL by EF, it wastefully performs a `SELECT *` followed by an implied loop where your lambda expression is finally called for each row returned.

It’s even easy to fall into this trap after performing a seemingly-safe refactoring. Consider the query:

```csharp
var williamson = context.Counties.Where(x => x.State == “TX” && x.Name == “Williamson”);
```

We might perform a seemingly safe refactoring, turning the lambda expression into a named C# method:

```csharp
var williamson = context.Counties.Where(x => IsWilliamsonTx(x));

...
 
private bool IsWilliamsonTx(County x)
    => x.State == “TX” && x.Name == "Williamson"
```

Unfortunately, this is now a `SELECT *`, where the condition is evaluated against each record on the C# side. There is no way that EF could possibly inspect our lambda expression in the second case to determine the equivalent SQL. The best it can do is `SELECT *` and evaluate your C# lambda row by row.