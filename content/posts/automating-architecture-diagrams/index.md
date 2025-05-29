---
title: "Automating Architecture Diagrams"
date: 2025-05-29T13:00:00+01:00
draft: false
summary: "I wanted to document my stock, go-to approach for creating and sharing software architecture diagrams. It’s simple, low-maintenance because it lends itself to automation, and good enough most of the time. It’s not clever or groundbreaking, but might be useful if you’re looking to improve your documentation."
images: []
resources:
- name: "featured-image"
  src: "featured-image.png"
tags: ["Architecture", "Continuous Integration", "Continuous Delivery"]
toc:
  enable: true
---

I have a stock approach I use as a baseline for creating and sharing architecture diagrams. It’s not big, and it’s not especially clever — but it is simple. It’s low-maintenance because it lends itself well to automation, and it’s good enough most of the time. To help you decide if it might be useful, I’ll walk through what I want to achieve and why.

## Wants

- I want to clearly communicate both the current state and a future vision of the system.
- I believe in doing [the simplest thing that could possibly work](https://wiki.c2.com/?DoTheSimplestThingThatCouldPossiblyWork). I don’t want to gold-plate or waste effort where it’s not needed.
- I want source control. I like backups. I like history, diff, merge — all that good stuff. I like docs that live alongside code.
- I don’t like people wasting time on things that computers could be doing.
- I like it when things [just work](https://www.youtube.com/watch?v=YPN0qhSyWy8) and teams can "fall into the pit of success"[^1].

## Shared Language

Architecture diagrams are a prop. A tool to support the development of shared understanding within a team. As you might expect from someone who advocates for Domain-Driven Design, I think it’s important to have a common visual language for discussing architectural concepts. With a common language, it becomes much easier to communicate complex ideas without misunderstanding or miscommunication.

While UML still exists — and it’s fine — it’s a bit of a chonky system. There are 14 different diagram types, each with specific symbology and rules about how they relate to each other. And UML might not be as widely understood as it once was.

I prefer the [C4 model](https://c4model.com/). It offers a small set of lightweight, hierarchical diagrams with consistent symbology. It's easy to learn, quick to draw, and easy to read — which makes it great for sharing system architecture with minimal faff.

## Diagrams as Code

I like diagrams-as-code tools for the same reason I like whiteboards: I can get ideas down at a pace much closer to how I think. I don’t have all day to mess around with alignment tools, smart connectors, snapping, grouping, layering, and whatever else Visio or Illustrator think I care about. Which I don’t. I just want to draw my boxes and arrows, and move on with all the other stuff I need to do.

For the uninitiated, a diagrams-as-code tool lets you describe a diagram using a specialized plain text grammar, then lay out and render it algorithmically.

Because the diagram source is just text, it plays nicely with source control and standard editing workflows. You don’t need to buy, install, or learn some byzantine visual diagramming tool — and more importantly, you don’t need to force anyone else to do that just to view or edit your diagrams.

There are a few of these tools. [Graphviz](https://graphviz.org/) sits at the simpler, more generic end of the spectrum, while [Mermaid.js](https://mermaid.js.org/) and [PlantUML](https://plantuml.com/) offer more specialized support for software architecture diagrams.

I like PlantUML. It supports C4 diagrams (among many others), and its Visual Studio Code extension includes live preview, export, and other quality-of-life features.

### PlantUML

Here is the source of a C4 context diagram from the [example repo](https://github.com/christopher-bimson/ArchitectureDocumentationDemo/blob/main/.diagrams/context.puml). You can also see the [rendered diagram](https://github.com/christopher-bimson/ArchitectureDocumentationDemo/blob/main/.images/context.svg).

{{< admonition type=Example title="PUML" open=true >}}

```
@startuml
    !include https://raw.githubusercontent.com/plantuml-stdlib/C4-PlantUML/master/C4_Context.puml

    LAYOUT_WITH_LEGEND()

    title System Context diagram for Internet Banking System

    Person(customer, "Personal Banking Customer", 
    "A customer of the bank, with personal bank accounts.")
    
    System(banking_system, "[[./container.svg Internet Banking System]]", 
      "Allows customers to view information about their bank accounts, and make payments.")

    System_Ext(mail_system, "E-mail system", "The internal Microsoft Exchange e-mail system.")
    System_Ext(mainframe, "Mainframe Banking System", 
      "Stores all of the core banking information about customers, accounts, transactions, etc.")

    Rel(customer, banking_system, "Uses")
    Rel_Back(customer, mail_system, "Sends e-mails to")
    Rel_Neighbor(banking_system, mail_system, "Sends e-mails", "SMTP")
    Rel(banking_system, mainframe, "Uses")
@enduml
```

{{< /admonition >}}

A couple of things worth noting: PlantUML diagrams are laid out using an algorithm. That’s usually adequate, but it’s not always especially aesthetic. If you need polished, public-facing diagrams, it’s worth having a real designer beautify them.

PlantUML also supports embedding hyperlinks using the following syntax:

```
[[https://example.com Label Text]]
```

This can be a useful trick for navigating between architecture diagrams or linking out to related documentation, provided that the diagrams are rendered as SVG. Exactly how well this works will depend on your hosting choices and browser behaviour.


## Automation

There are three steps to using PlantUML:

- Edit the diagram source.
- Render it to an image.
- Publish the image somewhere people can see it.

This process naturally lends itself to two things:

- Someone inevitably forgetting one (or more) of the steps.
- Automating the whole thing via a CI pipeline.

### Pipeline

A pipeline is pretty straightforward to set up. You can see an example GitHub Action in [the example repo](https://github.com/christopher-bimson/ArchitectureDocumentationDemo/blob/main/.github/workflows/render-diagrams.yaml).

{{< admonition type=Example title="GitHub Action" open=true >}}

```yaml
name: Render Diagrams

on:
  push:
    paths:
      - '**/*.puml'
  workflow_dispatch:

jobs:
  render:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout repository
        uses: actions/checkout@v3

      - name: Set up Java
        uses: actions/setup-java@v4
        with:
          distribution: 'temurin'
          java-version: '17'

      - name: Install Graphviz
        run: sudo apt-get update && sudo apt-get install -y graphviz

      - name: Download PlantUML
        run: curl -L -o plantuml.jar https://github.com/plantuml/plantuml/releases/latest/download/plantuml.jar

      - name: Render all .puml diagrams to ./images
        run: |
          mkdir -p .images
          find . -type f \( -path "./.diagrams/*.puml" -o -path "*/.diagrams/*.puml" \) | while read -r f; do
            filename=$(basename "$f" .puml)
            cat "$f" | java -jar plantuml.jar -tsvg -pipe > ".images/${filename}.svg"
          done

      - name: Commit and push rendered SVGs
        run: |
          git config user.name "github-actions"
          git config user.email "github-actions@users.noreply.github.com"
          git add .images
          git diff --cached --quiet || git commit -m "Auto-rendered PlantUML diagrams"
          git push
```

{{< /admonition >}}

A couple of tips:

- Make sure your diagram generation only runs when `.puml` source files change.
- Conversely, you probably want to exclude `.puml` changes from triggering your regular CI build.
- The example above publishes the rendered images by committing them back to the repository, where they are embedded in various `README.md` files. But documentation systems like:
  - Confluence  
  - Notion  
  - SharePoint (🤢) 
  
  have APIs you can use to push diagrams directly into their respective content stores.

[^1]: I first heard this phrase from my former colleague [Nathan](https://github.com/NathanLBCooper). No idea if he came up with it or stole it from somewhere.