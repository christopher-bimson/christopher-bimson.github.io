---
title: "Enforcing Architecture Constraints with ArchUnit"
date: 2024-07-11T13:00:07+01:00
draft: false
summary: "Relying on manually reviewed pull requests to ensure that architecture constraints are being followed? Don't. Instead, define the rules of your architecture and write unit tests to ensure they are not broken."
images: []
resources:
- name: "featured-image"
  src: "featured-image.jpg"
tags: ["Architecture", "Continuous Integration", "Continuous Delivery"]
toc:
  enable: false
---

I wrote in a [prior post](/2024/07/pull-request-theatre/) that I think that pull request-based workflows are wasteful, and many if not all of the checks that are performed manually in a review can be automated. One of the benefits teams seek from manually approving pull requests is to enforce architecture constraints. But what if you could write unit tests for your architecture?

## Introducing ArchUnit

ArchUnit is a library that allows you to define the layers in your architecture, create rules that describe what is and is not allowed, and to write unit tests to ensure these rules are obeyed.

ArchUnit is a [java library](https://www.archunit.org/) with a [.NET port](https://archunitnet.readthedocs.io/en/latest/). You can get it via the normal package managers in both ecosystems. In java, there are versions for both JUnit 4 and 5. In .NET, you will want to install an additional package integrating it with your preferred unit test framework. 

While it's not going to be super relevant to the rest of this post I want to mention the existence of [ts-arch](https://github.com/ts-arch/ts-arch) for the node.js ecosystem, which is inspired by ArchUnit but isn't really a port.

## Walkthrough 

[This example](https://github.com/christopher-bimson/ArchUnitDemo) demonstrates the basics of using ArchUnit. It's in C#, but the differences between the java and .NET versions of ArchUnit are small enough that it should work well enough to demonstrate the core concepts.

### The Basic Task List Service

The basic task list service is a simple RESTful API that allows a consumer to create simple to-do lists, which can be exercised via a built in swagger UI endpoint. It's fair to say that it does nothing especially useful, and is a touch overbuilt in order to make an effective demonstration. 

It follows a hexagonal-ish architecture as illustrated below.

{{< figure src="basic-task-list-component.svg" title="BasicTaskList Component Diagram" >}}

### Importing Code

The first step is to import the code for which you want to define layers and rules. In .NET the simplest way to do this is by specifying assemblies to load (the rough equivalent in java would be to import class files by package) like so:

```csharp
private static readonly Architecture Architecture = new ArchLoader()
    .LoadAssembliesIncludingDependencies([System.Reflection.Assembly.Load("BasicTaskList.Api")])
    .Build();
```

but there are other options that allow you to be more selective about what you import, such as filtering by namespace.

### Defining Layers

Layers are defined by identifying the types that make up the layer and giving it a name. In .NET the simplest way to include types in a layer is by namespace, as seen below:

```csharp
public static readonly IObjectProvider<IType> RestApi = ArchRuleDefinition.Types()
    .That().ResideInNamespace("BasicTaskList.Api.Controllers")
    .Or().ResideInNamespace("BasicTaskList.Api.Resources")
    .As("REST API layer");
```

There is an extensive API for identifying types that are part of a layer, and they can be combined in a [specification pattern](https://en.wikipedia.org/wiki/Specification_pattern)-style fluent interface. You could even enumerate all the types in the layer by hand if you really needed to.

### Specifying Rules

Rules are specified using an interface that will be familiar to anyone who has used libraries like [Fluent Assertions](https://fluentassertions.com/). In this example the rules mainly focus on the dependency relationships between layers, but they can also specify inheritance relationships, accessibility modifiers, the use of specific attributes (annotations in java) or many other properties. You can also describe the reason the rule exists, as shown below:

```csharp
var notDependOnRestApi = Types().That().Are(Layers.Model)
    .Should().NotDependOnAny(Types().That().Are(Layers.RestApi))
    .Because("the model should be ignorant of the application host(s) that consume it.");
```

### Writing Tests

Write an architecture test as you would any other unit test in your preferred framework. The only difference is how assertions are made, by using the `IArchRule.Check` method:

```csharp
[Fact]
public void Not_Permit_The_Model_To_Depend_Upon_The_RestAPI_Layer()
{
    var notDependOnRestApi = Types().That().Are(Layers.Model)
        .Should().NotDependOnAny(Types().That().Are(Layers.RestApi))
        .Because("the model should be ignorant of the application host(s) that consume it.");

    var notDependOnAspNetCore = Types().That().Are(Layers.Model)
        .Should().NotDependOnAny(Types().That().Are(Namespaces.MicrosoftAspNetCore))
        .Because("the model should be framework agnostic.");
    
    notDependOnRestApi.And(notDependOnAspNetCore).Check(Architecture);
}
```

You can combine rule checks using a similar specification pattern-style API. You can also see a rule here that is only implicit in the component diagram above - that our model should be independent of ASP.NET Core itself - which gives you an inkling of the kinds of thing you can do with ArchUnit.

### Feedback

If a test fails ArchUnit provides good feedback that will help you identify the problem that needs to be fixed. In the example below, I have deliberately broken the rule from the example above by including references to the ASP.NET `Controller` class in the application service layer:

```shell
ArchUnitNET.xUnit.FailedArchRuleException
"Types that are MVC model layer should not depend on Types that are Microsoft.AspNetCore 
 namespace because the model should be framework agnostic." failed:
	BasicTaskList.Api.Model.ApplicationServices.ListApplicationService 
    does depend on Microsoft.AspNetCore.Mvc.Controller
  BasicTaskList.Api.Model.ApplicationServices.TaskListReadModel 
    does depend on Microsoft.AspNetCore.Mvc.Controller  
```

The test output enumerates every rule break so you should not have any trouble figuring out what the problem is and fixing it. 

## Wrap Up

With ArchUnit, you can replace manual checks for architecture issues in pull requests by codifying your architectural rules and enforcing them through automated tests.

ArchUnit tests are just like any other unit test, meaning they can automatically fail your CI pipeline and prevent your architecture from degrading over time. Additionally, because these tests can be run locally, they provide much faster feedback than waiting for a pull request to be reviewed.