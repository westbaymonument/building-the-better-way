---
layout: default
title: Logging
nav_order: 6
parent: Development Guidelines
has_children: false
---

# Logging
{: .no_toc}

Logging at its simplest is recording useful information as various system events occur. Because logging is useful for handling multiple system concerns, the term is unfortunately overloaded and can be ambiguous. Some common concerns that involve logging are:

- **Diagnosing** - Verifying the application is running as expected, or determining why it isn't
- **Technical analysis** - detecting superfluous network calls, select n + 1 issues, etc.
- **Monitoring** - Alerting humans when abnormal conditions are detected in either software (e.g. unhandled runtime exceptions, excessive user login failures) or the underlying hardware (e.g. low disk space)
- **Business analysis** - Recording business events for analysis (e.g. average number of cups made per day)
- **Profiling** - Measuring the application's runtime performance and resource consumption
- **Auditing** - Recording what users do or attempt to do, to ensure security and detect unauthorized access

This document focuses on application diagnostics and monitoring of application runtime exceptions. 

There's a variety of tools available that either specialize in one particular facet (e.g. Prometheus for metrics) or provide singular management for multiple concerns. These tools should be used where appropriate, but keep in mind that simple needs may be achievable without additional tooling. For example, it may be easy to distill simple metrics such as cups produced per day from the number of "new cup produced" log messages. As another example, alerting sysadmins about system issues might be sufficiently covered by configuring log messages for errors to be emailed.

When architecting for logging, start by defining where the messages will be stored and how they will be accessed. This decision will potentially impact what library gets chosen to record and dispatch log messages to the store.

- TOC
{:toc}

## How to Select a Logging Store

### **DO** assess whether the project would benefit from a centralized logging store
{: .text-green-100 }
Most single application projects will have a “centralized” store by default, by using either the file system or database of the web server. A distributed system, on the other hand, will require a “real” centralized store that is on the network and receives transmissions from multiple sources.

Highly consider using a network centralized store if:

- The project is a distributed system, and correlating “what happened” across individual log stores from multiple services would be painful.

Consider using a centralized cloud store (e.g. Splunk, Application Insights) if:

- The project would benefit from infrastructure monitoring and alerts. Most products provide a way to additionally receive application diagnostic messages, providing a single store for application events and infrastructure performance.

Consider using an on-premise centralized store (e.g. ELK) if:

- The application needs to operate without internet. (e.g. manufacturing facility)
- There are concerns about semi-sensitive operating metrics being in the cloud. (you should not be logging any highly sensitive information like Social Security Numbers, credit card numbers, passwords, etc.)

### **DO** plan for all stakeholders involved when selecting a logging store
{: .text-green-100 }
Make a list of all the roles that need to access logs, or to receive notifications. Typical candidates are:

- Developers
- QA
- IT support
- Outside auditors

Ensure that the chosen logging sink can be accessed by everyone who needs it. You may also find that groups should have varying access to particular types of entries.

### **DO** define business concepts and metrics to be logged when clear value is present
{: .text-green-100 }
It can be much harder to do meaningful analysis on properties that aren’t being captured. Similar to reports, drive conversations of “what do you wish you could more easily know?” and “what decisions would you make based on that knowledge?”

### **DO** assess expected costs
{: .text-green-100 }
Do preliminary estimates of data storage if possible. This may be admittedly difficult at the beginning, if the product charges by traffic volume. Definitely estimate any licensing or per node/per person costs.

### **DO** analyze and document retention policies
{: .text-green-100 }
Analyze any log data that will need to be recorded and retained to meet regulatory compliance. Account for this in storage logistics and costs.

For metrics that generate a lot of fine grained data, consider migrating data to a lower resolution after a certain time period (every month, quarter, year, etc.).

### **CONSIDER** using Application Insights
{: .text-yellow-300 }
Application Insights integrates well with the Microsoft frameworks and Azure infrastructure when utilized as a cloud provider.

## How to Select A Logging Library

### **DO** use Serilog as a “default” choice if no existing tool is in use
{: .text-green-100 }
[Serilog](https://serilog.net/) was built from the ground up to support structured logging, and has many contributed sinks. 

[NLog](https://nlog-project.org/) is another solid choice that at one time was eclipsed by Serilog (no structured logging, slower to support .NET Core). Most of those gaps have been closed. Today, there is still one differentiator though - Serilog supports enrichers, and NLog does not.

## How to Architect the Configuration of Logging

### **DO** ensure logging can be configured without redeploying binaries
{: .text-green-100 }
At a minimum it should be possible to adjust the logging level in an environment without doing a deployment to respond to potential performance issues from verbose logging. Doing this means externalizing the logging level to configuration files.

See [Serilog configuration](https://github.com/serilog/serilog/wiki/Configuration-Basics) for links to .NET full and .NET Core framework extensions for file configuration.

If you’re using ASP.NET Core, it provides a configuration options capability that will be applied without restarting the app, see the [options pattern documentation](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/configuration/options?view=aspnetcore-3.1) for details.

### **CONSIDER** using code for common configuration
{: .text-yellow-300 }
Many systems will have the same sink types (e.g. database, file path, ELK stack) with different instances or locations for the various environments (e.g. dev, prod). If a particular sink is used in all environments (including local development), consider configuring that sink in C# code and only storing the instance configuration (connection string, file path, IP address) in configuration files.

### **CONSIDER** using Microsoft Logging Extensions for ASP.NET Core applications
{: .text-yellow-300 }
ASP.NET Core uses the new logging abstraction Microsoft.Logging.Extensions. Using this abstraction has two benefits:

* ‘Future proofing’ against a future change in logging tools
* Ability to capture relevant ASP.NET Core log messages in your configured sinks

## How to Add Logging Messages

### **DO NOT** use tooling to automatically add diagnostic and business analysis logging messages
{: .text-red-300 }
While developers will save a small amount of time by not creating logging statements, automatically generated logging statements are implemented without any human thought for usefulness. Your logs will be a runtime stack trace.

Note this guidance is specifically for diagnostic and business analysis logging. Injecting instrumentation automatically can be useful. Consider using an external profiling tool that can do the injection itself to avoid a two step profiling process (inject with one tool, profile with another).

## When to Add Logging Messages

### **DO** log unhandled errors
{: .text-green-100 }
For .NET full framework thick client, subscribe to `AppDomain.UnhandledException`.

For ASP.NET, add an exception filter.

See [Fatal](/development-guidelines/logging/index.html#fatal) and [Error](/development-guidelines/logging/index.html#error).

### **DO** log conditions you aren’t expecting but want to know if they occur
{: .text-green-100 }
See [Warn](/development-guidelines/logging/index.html#warn).

### **CONSIDER** logging business events for later analysis
{: .text-yellow-300 }
Generally you should have a centralized logging store and be actively collaborating with your users in defining these events.

See [Info](/development-guidelines/logging/index.html#info).

You have a backend process responding to external events or doing a periodically scheduled task. This can be a standalone process or a node in a distributed system.

### **DO** log complex value calculations or conditional logic
{: .text-green-100 }
The complexity may be isolated in a backend process, be it could also be in a UI interaction. Being able to record “what happened” to determine or verify behavior is useful.

See [Debug](/development-guidelines/logging/index.html#debug).

### **DO NOT** log trivial messages that can be determined from other sources
{: .text-red-100 }
For example, don’t log controller action entries that can be determined from web server logs.

## What To Include in Logging Messages

### **DO** use correlation ids in a distributed system or complex process
{: .text-green-100 }
Correlation ids can be used to group independently related messages together for a human to easily see associated together during analysis. For example, in a distributed system where service _A_ sends a request to service _B_ as part of an operation, including the same correlation id for events on services _A_ and _B_ makes it easy to see the chain of events from a system perspective, instead of having to manually link up the messages from each service individually.

A system does not have to be distributed for correlation ids to be useful. For example, a multistep business transaction in a single system can also benefit from the use of correlation ids.

## How to Choose a Level for Log Messages

### **DO** use appropriate logging levels
{: .text-green-100 }
#### Fatal

The app or service cannot continue running and is either self-terminating or being terminated by the operating system.

#### Error

An operation (ASP.NET controller action, thick client UI button click event handler, periodic timed process, message bus handler) unexpectedly failed, but the app or service can continue doing other operations.

Clarification - an expected validation is not an error. If you have an operation that requires a last name to be specified, and your operation detects it missing and returns “last name required”, this is NOT a message that should be logged at the error level. (on the other hand, if one of your operational goals is to measure usability or user accuracy, you might log these as a variety of INFO metric)

#### Warn

Something atypical occurred, but the operation can still be successfully completed.

- A condition assumed to be impossible has happened, such as “all of the invoices will be approved before the invoice finalizer is automatically invoked”. A human should look at how the “impossible” happened.
- An anticipated problem was handled, but repeated occurrence could be a problem.
- Initial call to external system failed but retry succeeded
- Circuit breaker pattern tripped

#### Info

This is usually the default level for environments other than local development. The volume of INFO messages can represent a large quantity of information in a production environment, but enabling INFO level in production should cause no concern for significant performance impact or sink capacity.

**General**

- App or service startup events and configuration
- App or service shutdown events
- Changes to app or service general configuration

**Business**

Only do this if you’re using a centralized store and working with the business to identify business events to be logged for analysis. An example interest point might be tracking the average number of cups produced per hour to correlate against other factors like average order size.

**Headless events that change state**

Only record these yourself if your hosting service does not. For example, don’t log ASP.NET controller actions being invoked, use the web server log instead.

**Significant, infrequently (less than once per hour) scheduled events**

Keeping a record of these allows an easy check on whether expected processing is occurring or not.

**Significant, frequently scheduled events**

Do not log frequently occurring events at the INFO level to avoid log flooding. Consider adding a less frequent “heartbeat” message instead.

#### Debug

A code path has significant complexity in calculation or conditional branching. Add enough debug messages that you can trace what is happening without attaching a debugger. 

### **AVOID** logging sensitive information
{: .text-red-300 }
Ensure your logger can omit credentials and/or mask sensitive information.  For example, never log a complete credit card number but rather mask all but the last four digits.

### **DO** tune the log level for 3rd party packages
{: .text-green-100 }
Monitor your logs for verbosely logging packages and tune using configuration to match the log level of your application code consistently.

## How to Configure Application Logging

### **DO** set the appropriate log level in deployed environments
{: .text-green-100 }
The typical default level for nondevelopment environments is Info. More verbose logging should only be used in limited situations to isolate known issues.

### **CONSIDER** overall sink cost to process messages
{: .text-yellow-300 }
Know the cost of your chosen sink solution (e.g. cloud hosted transactional database) in terms of log message serialization (CPU), network consumption and latency, message deserialization (memory), message writes (e.g. indexing), message reads, and storage (disk cost).

### **DO** allow logging solution to gather metadata
{: .text-green-100 }
Logging the thread name, source filename, method name and other message metadata should be handled by the logging library.  Avoid making added calls that impact CPU and I/O to gather additional logging output.

### **DO** make use of buffering and throttling to protect the performance of your system
{: .text-green-100 }
Be aware if your logging solution (and sink) support either buffering (queueing messages internally and writing in blocks, such as a file sink) or throttling (a network packet forwarder that can constrain itself to prevent overloading the central store) and enable this feature.  Robust protections can throttle per message source and/or message type and indicate in viewed output that protections are in-effect during logging spikes.

## Log level guidance

From [Microsoft](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/logging/?view=aspnetcore-2.2#log-level)

From [Serilog](https://github.com/serilog/serilog/wiki/Configuration-Basics#minimum-level)

## Appendix 1: Logging Library Selection Criteria

This section documents the qualities and capabilities to look for in a logging library.

**Configurable**

Can the logger be configured at runtime with a variety of output sinks? We should be able to code “log.Debug” and never have to adjust it whether we are saving to a file, database, or network store.

**Sufficiently performant**

Enabling logging should not cause a noticeable performance slowdown in the system.

**Non-throwing**

Your application should never experience an exception or error because the logging library experiences an issue. (e.g. the disk being written to is full)

**Well maintained**

Preferably the library is community or commercially supported and not home grown. It should be actively developed with regular updates, stable, and have a healthy ecosystem of integrations, plug-ins, extensions with other tooling such as Microsoft common logging, DI tooling, etc.

**Supports Structured Logging**

Structured logging differs from traditional text-based logging by capturing messages as a series of timestamped name/value properties, instead of strings. A traditional log message might be:

```
“Login failed. Number of attempts: 4”
```

A structured log message could conceptually be visualized as:

```
Time: 2019-06-07 12:34

Message: “Login failed.”

Properties:

    Key: Number of attempts

    Value: 4
```

Because messages for events have well structured values (the value “4” for the property “number of attempts”), they can be queried with relational operators (e.g. show me all failed login events with number of attempts greater than 3). This is usually not easy with a pure string of messages.
