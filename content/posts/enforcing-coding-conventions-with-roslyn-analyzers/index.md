---
title: "Enforcing Coding Conventions with Roslyn Analyzers"
date: 2024-07-21T13:00:07+01:00
draft: false
summary: "Relying on manually reviewed pull requests to ensure that coding conventions are being followed? If you live in .NET land, don't. Use custom Roslyn analyzers to spot and fix issues ahead of your CI pipeline."
images: []
resources:
- name: "featured-image"
  src: "featured-image.jpg"
tags: [".NET", "Continuous Integration", "Continuous Delivery"]
slug: "/enforcing-conventions-with-roslyn-analyzers/"
toc:
  enable: true
---

This is the last in a mini-series of posts about automating checks performed by humans during code reviews, in order to [remove wasteful blocking pull requests](/2024/07/pull-request-theatre/) from your teams' workflow. 

Does your team have a list of conventions in a 'things to look for during reviews' article on their wiki? Or perhaps in a [pull request template](https://docs.github.com/en/communities/using-templates-to-encourage-useful-issues-and-pull-requests/creating-a-pull-request-template-for-your-repository)? If you are using .NET there is a way to automate the kind of checks that pop up in these lists.

## Roslyn Analyzers

Roslyn is the codename for the open-source, self-hosted compilers for C# and VB.NET, along with APIs for building code analysis tools. 

Analyzers are extensions to the compiler that take advantage of these APIs to detect and surface issues in source code. They can even suggest refactorings to remediate issues that can be taken advantage of by a suitable editor or IDE.

## Playing By Our Own Rules

To demonstrate the kind of things that analyzers can do I am going to define some simple coding conventions that represent the kind of things that a C# team may want to follow.

1. No use of `null`. Prevent developers falling victim to the [billion dollar mistake](https://youtu.be/YYkOWzrO3xg?si=keBy_JjIOsirP5Vw).
2. Always use coordinated universal time when the current date or time is required.
3. Don't use a static reference to get the current date or time, as that makes time sensitive logic hard to test.

## A Sample Analyzer

You can find the example project that implements analyzers for these rules on [GitHub](https://github.com/christopher-bimson/RoslynAnalyzerDemo). I'll walk through the implementation of a single analyzer, but I encourage you to clone the repository and explore the implementation in more detail.

### Getting Started

Analyzers must be a subclass of `Microsoft.CodeAnalysis.Diagnostics.DiagnosticAnalyzer` in the `Microsoft.CodeAnalysis` assembly. They also need to be decorated with the `DiagnosticAnalyzer` attribute specifying the language that the analyzer applies to.

```csharp
using Microsoft.CodeAnalysis;
using Microsoft.CodeAnalysis.Diagnostics;

namespace Demo.Analyzers.Dates;

[DiagnosticAnalyzer(LanguageNames.CSharp)]
public class LocalNowUsageAnalyzer : DiagnosticAnalyzer
{
  // Detail omitted.
}
```

### Implementing the Interface

There are two abstract members that need to be implemented. `SupportedDiagnostics` should be an immutable array containing instances of the diagnostic messages that the analyzer can raise. In this case we only need a single diagnostic, a warning that explains to use `DateTime.UtcNow` instead of `DateTime.Now`.

`Initialize` is called to setup the analyzer for use in a specific context. Here we tell the context that:

- We don't want to run the analyzer against generated code.
- The analyzer supports concurrent execution
- We register a method, `AnalyzeSyntax` to be called when the compiler encounters a simple member access expression. It is this method that will detect the issue we are looking for.

```csharp
using System.Collections.Immutable;
using Microsoft.CodeAnalysis;
using Microsoft.CodeAnalysis.CSharp;
using Microsoft.CodeAnalysis.CSharp.Syntax;
using Microsoft.CodeAnalysis.Diagnostics;

namespace Demo.Analyzers.Dates;

[DiagnosticAnalyzer(LanguageNames.CSharp)]
public class LocalNowUsageAnalyzer : DiagnosticAnalyzer
{
    internal const string Id = "DEM003";

    private static readonly DiagnosticDescriptor Descriptor = 
        new DiagnosticDescriptor(Id, 
            "Detects usage of DateTime.Now", 
            "Don't use DateTime.Now. Use DateTime.UtcNow instead.", 
            "Usage", 
            DiagnosticSeverity.Warning, 
            isEnabledByDefault: true, 
            description: "Warns if you are using DateTime.Now.");

    public override ImmutableArray<DiagnosticDescriptor> SupportedDiagnostics => ImmutableArray.Create(Descriptor);

    public override void Initialize(AnalysisContext context)
    {
        context.ConfigureGeneratedCodeAnalysis(GeneratedCodeAnalysisFlags.None);
        context.EnableConcurrentExecution();
        context.RegisterSyntaxNodeAction(AnalyzeSyntax, SyntaxKind.SimpleMemberAccessExpression);
    }

    private void AnalyzeSyntax(SyntaxNodeAnalysisContext context)
    {
        // Detail Omitted
    }
}
```

### Detection

Here you can see the implementation of the `AnalyzeSyntax` method that completes the analyzer. It simply gets the current syntax node from the provided context, executes a simple predicate to determine if the expression is `System.DateTime.Now`, and if so reports a new diagnostic message using the descriptor we defined earlier. 

```csharp
using System.Collections.Immutable;
using Microsoft.CodeAnalysis;
using Microsoft.CodeAnalysis.CSharp;
using Microsoft.CodeAnalysis.CSharp.Syntax;
using Microsoft.CodeAnalysis.Diagnostics;

namespace Demo.Analyzers.Dates;

[DiagnosticAnalyzer(LanguageNames.CSharp)]
public class LocalNowUsageAnalyzer : DiagnosticAnalyzer
{
    internal const string Id = "DEM003";

    private static readonly DiagnosticDescriptor Descriptor = 
        new DiagnosticDescriptor(Id, 
            "Detects usage of DateTime.Now", 
            "Don't use DateTime.Now. Use DateTime.UtcNow instead.", 
            "Usage", 
            DiagnosticSeverity.Warning, 
            isEnabledByDefault: true, 
            description: "Warns if you are using DateTime.Now.");

    // Detail Omitted

    private void AnalyzeSyntax(SyntaxNodeAnalysisContext context)
    {
        var memberAccessExpr = (MemberAccessExpressionSyntax)context.Node;

        if (!IsDateTimeNow(context, memberAccessExpr)) 
            return;
        
        var diagnostic = Diagnostic.Create(Descriptor, memberAccessExpr.GetLocation());
        context.ReportDiagnostic(diagnostic);
    }

    private static bool IsDateTimeNow(SyntaxNodeAnalysisContext context, MemberAccessExpressionSyntax exp)
    {
        return exp.Name.Identifier.Text == "Now" &&
               context.SemanticModel.GetTypeInfo(exp.Expression).Type?.ToDisplayString() == "System.DateTime";
    }
}
```

### Delivery

The best way to distribute and use Roslyn analyzers is by building a NuGet package. For this to work correctly be sure that the assembly containing the analyzers is packaged into the `analyzers/dotnet/cs` folder like so:

```xml
<ItemGroup>
  <None Include="bin\$(Configuration)\$(TargetFramework)\Demo.Analyzers.dll" Pack="true" 
        PackagePath="analyzers/dotnet/cs" Visible="false"/>
</ItemGroup>
```

## Usage

Usage is simply a matter of adding a reference to the NuGet package. I've written a simple one-file console application with code that breaks these rules. This allows you to see the feedback in action.

### UI

UI feedback will vary slightly by the editor or IDE in use. Here are some examples of what the feedback looks like in the Rider IDE.

{{< figure src="null-rule-feedback.png" title="Example feedback for the null usage rule." >}}

{{< figure src="datetime-now-feedback.png" title="Example feedback for the DateTime.Now usage rule." >}}

### Compiler Output

Compiler output will look the same for all tools.

```console
C:\Program.cs(16,12): warning DEM002: Don't use nullable types. Use an Option type like https://github.com/nlkl/Optional or https://github.com/ mcintyre321/OneOf. [C:\Demo.csproj]
C:\Program.cs(1,36): warning DEM004: Don't use DateTime.UtcNow. Use an injectable abstraction that can be replaced with a test double instead.  [C:\Demo.csproj]
C:\Program.cs(12,23): warning DEM003: Don't use DateTime.Now. Use DateTime.UtcNow instead. [C:\Users\chris.bimson\Documents\Personal\AnalyzerDemo\Demo\Demo\Demo.csproj]
C:\Program.cs(6,31): warning DEM002: Don't use nullable types. Use an Option type like https://github.com/nlkl/Optional or https://github.com/m cintyre321/OneOf. [C:\Demo.csproj]
C:\Program.cs(6,52): warning DEM001: Don't use null. Use an Option type like https://github.com/nlkl/Optional or https://github.com/mcintyre321/OneOf. [C:\Demo.csproj]
    5 Warning(s)
    0 Error(s)
```

## Strengths

Any analyzer you create will provide feedback in the normal coding workflow, faster even than CI, which gives developers the opportunity to remediate issues immediately. If the issue is simple then it might be possible to implement a refactoring (as in the [example project](https://github.com/christopher-bimson/RoslynAnalyzerDemo)) to fix it automatically.

Diagnostic messages from analyzers appear in the compiler output, providing some useful flexibility. The compiler output is an integration point with [static analysis](https://www.sonarsource.com/products/sonarcloud/) tools. A shared `.editorconfig` file can be used to change the default severity of diagnostics, giving a team the ability to tailor the diagnostics provided by a library of analyzers to their context. Additionally, for situations where following a convention would be worse than breaking it, you have the option to suppress warnings reported by analyzers using a `#pragma` directive.

## Verses ArchUnit

As [I wrote previously](/2024/07/enforcing-architecture-constraints-with-archunit/) ArchUnit can be used to unit test architecture constraints and so performs a similar quality check function in the CI pipeline to a set of custom Roslyn analyzers. ArchUnit works best when examining the properties of and relationships between object code across an entire project or code base. The sweet spot for analyzers seems to be identifying issues in source code without context. 

Rather than being alternatives the two approaches complement each other and I would recommend employing both to build a robust, rigorous CI pipeline that doesn't need to rely on blocking pull request reviews to ensure a high level of quality.

