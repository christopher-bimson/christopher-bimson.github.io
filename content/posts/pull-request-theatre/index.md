---
title: "Pull Request Theatre"
date: 2024-07-07T12:52:09+01:00
draft: false
summary: "I expand on a conversation I had a couple of months ago, where I scoffed at workflows that manually approve pull requests before merging them without really explaining why."
images: []
resources:
- name: "featured-image"
  src: "featured-image.jpg"
tags: ["Continuous Delivery"]
toc:
  enable: false
---

A couple of months ago I had a [conversation](https://timwise.co.uk/2024/04/18/trunk-based-development-is-wrong/) with my friend Tim about [trunk-based development](https://trunkbaseddevelopment.com/) that ended up spanning feature branches, Git history, feature toggles and related stuff. At one point it was implied that development workflows where pull requests are manually reviewed and need to be approved to merge are optimal. I didn't agree and glibly described this approach as 'mostly theatre' but I didn't really explain why. So here's why.

# Pull Request Workflows

I'm not talking about pull requests as a standalone feature of collaborative development platforms like GitHub, GitLab, and Azure DevOps. I mean any workflow where team members other than the author provide feedback, request changes, and are required to approve pull requests for merging into the main or master branch. 

# Why Theatre?

[Bruce Schneier](https://www.schneier.com/) coined the term security theatre to describe measures that give the _impression_ of effective security but are ineffective at addressing or mitigating _actual_ vulnerabilities. Security theatre values appearances over effectiveness.

Pull request workflows are the software development equivalent of security theatre, prioritizing ritual and appearance over effectiveness.

## Longer Feedback Loops

Any feedback developers receive in a pull request workflow arrives relatively late in the development process. In the worst case, it only arrives when a developer considers their work 'complete' and ready to merge. Late feedback issues makes rework more likely. 

Rework is a wasteful overhead that can be frustrating and demoralizing for developers who might well feel their time has been wasted. Earlier and more frequent feedback can address issues before they accumulate and prevent large amounts of wasteful, demotivating rework.

A more insidious problem is when feedback is ignored or minimized due to the [sunk cost fallacy](https://thedecisionlab.com/biases/the-sunk-cost-fallacy), where developers might be reluctant to make changes after investing significant time and effort, even when the feedback is valid and beneficial.

## Delayed Integration 

No matter how aggressively your team throws themselves into PRs as they are created there will likely be a delay before reviewers are available, then there is the time to perform the review itself. This delay works against the goals of continuous integration. You want to integrate your work early and often to reduce the risk of introducing issues during integration and shorten the time it takes to the latest version of your software in to the hands of people who can provide valuable feedback.

## Context Switching

Most leaders and managers know about the [impact](https://www.joelonsoftware.com/2001/02/12/human-task-switches-considered-harmful/) of [context](https://blog.codinghorror.com/the-multi-tasking-myth/) [switches](https://www.apa.org/topics/research/multitasking) in knowledge work by this point. Developers _feel_ it viscerally so don't be surprised if they delay switching tasks for as long as they can to maintain their focus. This will have an impact on how quickly PRs can be reviewed in addition to the classic impact of context switching to a PR and then back to the reviewers original task.

## Inconsistent Reviews

Different reviewers can have specialties in different technologies, different areas of a codebase or prioritize different issues when reviewing and this can lead to an uneven standard of scrutiny. A good team can mitigate this by building a strong shared understanding of 'what good looks like', but factors like the relationship dynamics between team members can still affect the consistency of reviews.

## Superficial Reviews

No practice or processes is especially effective when executed incorrectly, so this is a cheap criticism. But don't tell me you have never seen the approve button clicked because "we are really in a hurry", "it's nearly lunch time" or "it's friday afternoon."

## On Long Running Feature Branches

[Long running - multiple days and longer - feature branches](https://nvie.com/posts/a-successful-git-branching-model/) are not a problem unique to this type of workflow, but they are an aggravating factor that makes every issue I described worse by encouraging larger units of work with more integration effort and even longer feedback loops.

# So if they suck, why does everyone use them?

There are a few potential benefits to be had from manually reviewing and approving pull requests:

* Enforcing constraints and standards. This can include coding standards, the appropriate use of design patterns,  architecture constraints or anything else the team cares about.

* Defect detection.

* Knowledge sharing. This covers outcomes from building team resilience by increasing the bus factor in specific areas to onboarding and training new team members.

Not every team is necessarily consciously looking for all of these outcomes, and don't underestimate the role of simple inertia in the choice of practices.

# But I want these benefits, what should I do?

This would be a pretty pointless post if there _weren't_ alternative ways to realize these benefits. Here is an overview of what I would recommend you do instead.

## Continuous Delivery with Rigorous Automated Checks

If your CI pipeline is green, you should be able to deploy your software to production with no human intervention beyond saying 'Go!'. Production deployments should be frequent, boring and mundane events. This means that your standards, constraints and your software's correctness must be enforced automatically by your pipeline implementation, while still being quick enough to enable a fast, effective feedback loop.

Fortunately there are plenty of tools available to enable this. I'm sure everyone is familiar with automated testing frameworks for their platform that can be used to verify the most important functional scenarios and cross functional properties of your software. Linters and similar tools can be used to check coding standards. Static analysis tools like [SonarCloud](https://www.sonarsource.com/products/sonarcloud/) and [Codacy](https://www.codacy.com/) can identify a range of defects. Dynamic analysis tools like [ZAP](https://www.zaproxy.org/) can help find security vulnerabilities. Tools like [Renovate](https://docs.renovatebot.com/) and [Dependabot](https://github.com/dependabot) can help manage dependencies.

Well designed CI and CD pipelines can not only deliver the outcomes that pull request reviewers do, they can do it faster, more rigorously and more consistently.

## Pair and Mob Programming

How do you keep the knowledge sharing and defect prevention benefits of code review? Turn it up to eleven and do it continually for the fastest possible feedback. There is more to ensemble programming than just chucking a bunch of people in an office or a zoom call and letting them get on with it, however.

To get started with pair programming I would recommend following [Llewellyn Falco's strong pair programming](https://llewellynfalco.blogspot.com/2014/06/llewellyns-strong-style-pairing.html) style. I'd also recommend you rotate your pairs every day or two so everyone on a team works together at least once every week or so.

If you want to get into mob programming check out [this post](https://www.agilealliance.org/resources/experience-reports/mob-programming-agile2014/) from Woody Zuill.

## Trunk Based Development

I said mean things about long running feature branches earlier. Trunk based development is the antidote to that particular poison. At it's simplest, everyone on the team pushes their commits to the main (trunk) branch. Larger teams should use short lived lived feature branches that are merged to main then deleted. **Short lived** is the key qualifier here. Feature branches should not survive longer than a day or so, and should only contain the work of one person or pair.

Successful trunk based development implies adopting a number of other practices, some of which I have mentioned already, but most I have not. 

Read up on the other things you need to do at [trunkbaseddevelopment.com](https://trunkbaseddevelopment.com/).

# So, I should do away with PRs?

Maybe. Maybe not. A lot of the tools that facilitate the rigorous checks in the CI pipeline I described earlier either assume the existence of PRs, or are easier to implement with PRs. You will _probably_ find it easier to build a pipeline that automatically merges a PR to main when the pipeline succeeds than it will be to build one that reverts a commit that fails the same checks. I'd be happy with either.

# TL;DR

It _might_ be a little hyperbolic to describe workflows with manually gated merges to main as theatre, but most (if not all) of the benefits of manual reviews can be provided by rigorous automation and workflow improvements. Any further marginal benefit provided by gated reviews probably won't be worth the additional cost of delay.

Keep an eye on the [Continuous Delivery](/tags/continuous-delivery/) tag as I'll be posting a couple of examples of things you can do to smarten up your CI pipeline.




