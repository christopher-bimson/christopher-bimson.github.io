---
title: "Architecture Reading List: 2021 Edition"
date: 2023-07-28T19:00:00+01:00
draft: false
summary: "In response to a question at work a few years ago I wrote a short software architecture reading list. It's not comprehensive and could be more up to date, but it's proven useful over the last couple of years as an answer to 'where do I even start?' type questions."
images: []
resources:
- name: "featured-image"
  src: "featured-image.jpg"
tags: ["Architecture", "Recommended Reading"]
toc:
  enable: true
---

{{< admonition type="abstract" open=true >}}
I wrote this in response to Slack question about a basic software architecture reading list at a consulting gig in 2021. I've shared it a few times since in response to similar questions. Someone recently pointed out to me that I apparently promised to post it on an old episode of the [Codurance Talks](https://codurancetalks.podbean.com/) podcast.

So, better late than never I guess? I haven't made any updates beyond what was required to post it in this format. 

I like to think it is still useful as an answer to the question 'where do I even start?' but arguments about what is 'essential' material for a list like this could go on forever. I'm sure I'll update it at some point.
{{< /admonition >}}

## Technology

Material about specific technologies you can find and pick up as you need. I'd recommend being familiar with at least one of the two biggest public cloud platforms, [AWS](https://docs.aws.amazon.com/index.html) or [Azure](https://learn.microsoft.com/en-us/azure/?product=popular). 

While they are not the same, many concepts are transferable to equivalents on the other platform. I find that that the details move so fast that books can quickly become useless, and prefer to rely on the official documentation.

Both [Microsoft](https://learn.microsoft.com/en-us/azure/architecture/browse/) and [Amazon](https://aws.amazon.com/architecture/) provide reference architectures for many common scenarios, and these are a not a bad way to learn.

## Continuous Delivery

[Accelerate](https://www.goodreads.com/en/book/show/35747076-accelerate) is a book that explains what capabilities matter for creating and delivering software and how to measure them.

[Continuous Delivery](https://www.goodreads.com/book/show/8686650-continuous-delivery?ac=1&from_search=true&qid=l3PWWCBXER&rank=1) explains how to go about achieving most of those capabilities.

## Domain Driven Design

Domain driven design isn't an architecture style but a way of working. Strategic domain driven design in the best way I know of to identify coherent, loosely coupled modules with well defined boundaries, which are the foundation of all good architectures.

[Domain-Driven Design: Tackling Complexity in the Heart of Software](https://www.goodreads.com/book/show/179133.Domain_Driven_Design) is the founding work on the topic

Many people find Vaughn Vernon's books, [Domain Driven Design Distilled](https://www.goodreads.com/book/show/28602719-domain-driven-design-distilled) and [Implementing Domain Driven Design](https://www.goodreads.com/book/show/15756865-implementing-domain-driven-design) easier going.

## Principles & Patterns

[Clean Architecture](https://www.goodreads.com/book/show/18043011-clean-architecture) is a good, basic introduction.

Simon Brown also has a volume 2 of [Software Architcture for Developers](https://www.goodreads.com/en/book/show/33221518-software-architecture-for-developers) that focuses on documentation and diagrams. I'd start by reading about his [C4 Model](https://c4model.com/) online and then move onto the book if you feel the need.

[Building Evolutionary Architectures](https://www.goodreads.com/en/book/show/35755822-building-evolutionary-architectures) is a good, high level discussion of designing for changeability.

[Patterns of Enterprise Application Architecture](https://www.goodreads.com/book/show/70156.Patterns_of_Enterprise_Application_Architecture) and [Enterprise Integration Patterns](https://www.goodreads.com/en/book/show/85012.Enterprise_Integration_Patterns) are good catalogues of solutions to common architecture problems.

## Microservices 

Microservice architectures are currently popular, but the term is frequently misused to describe things that are not microservices and many people are jumping on the bandwagon without understanding the tradeoffs.

Sam Newman's books [Building Microservices](https://www.goodreads.com/book/show/22512931-building-microservices) and [Monolith to Microservices: Evolutionary Patterns to Transform Your Monolith](https://www.goodreads.com/book/show/44144499-monolith-to-microservices) are good and explain why microservices are not a golden hammer suitable for all situations.

While not strictly about microservices the [Twelve-Factor App](https://12factor.net/) is a good set of principles for designing cloud-native applications.