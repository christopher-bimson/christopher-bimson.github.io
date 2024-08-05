---
title: "McProductivity"
date: 2024-08-04T09:00:07+01:00
draft: false
summary: "Roughly a year ago McKinsey and Company published an article entitled 'Yes, you can measure software developer productivity'. It was a controversial piece, but did McKinsey introduce a genuine innovation?" 
images: []
resources:
- name: "featured-image"
  src: "featured-image.jpg"
tags: ["Leadership", "Delivery Management"]
toc:
  enable: true
---

Roughly a year ago, McKinsey and Company, the consulting megacorp, published an article entitled [Yes, you can measure software developer productivity](https://www.mckinsey.com/industries/technology-media-and-telecommunications/our-insights/yes-you-can-measure-software-developer-productivity#/).

The article stirred up quite a commotion. McKinsey, not a name you would normally associate with software development thought leadership, contradicted the widely held belief that it isn't really possible to [measure the productivity of software development or developers as individuals.](https://martinfowler.com/bliki/CannotMeasureProductivity.html)

Always with my finger on the pulse, I wanted to share my thoughts on this even though it's long, long after everyone else has weighed in. Did McKinsey introduce a genuine innovation? Did they blow open a developer conspiracy to spend all their time [compiling?](https://xkcd.com/303/)

Short answer: No. Not really.

## Productivity

At it's simplest productivity is the measure of the efficiency of a system in converting inputs into value.

For example, economists compare countries by their labour productivity, which is the total number of hours worked in a national economy divided by its gross domestic product (GDP). If a country generates more GDP from the same amount of labour, it's productivity is improving.

Another simple example is a factory that produces generic brand widgets. I put £1000 of steel into the factory and it stamps out £2000 worth of widgets. If I invest in machines that waste less steel, I might be able to produce £3000 worth of widgets for the same £1000 of steel.[^1] 

## Why Does Anyone Care About Measuring Productivity Anyway?

This question is a more perceptive that it may first appear, and we'll come back to it. Assuming good faith[^2] and generalizing a bit, organizational leadership is accountable for getting the most value out of the investment made in that organization. Therefore, they might ask:

* How can I get better outcomes for the same level of investment?
* If I have to cut spending on development by 10%, can we still meet our goals for this period?
* I have an extra £1000000 to invest, should it go into more software development, a marketing campaign, or a shiny new office?
* How do I figure out who to reward to incentivize performance?

These are reasonable questions for organizational leaders. In any technical leadership position, shrugging and answering "Yeah, it'd be nice to know that. Too bad, eh?" isn't good enough. You are inviting the kind of dysfunction we are about to delve into.

## The Garbage Fire

There is a lot to critique in the McKinsey article. Rather than fisk it line by line I'll focus on what I think matters the most.

### Activity Is Not Value

As mentioned earlier, productivity is about the efficiency of turning inputs into value. In most businesses this will eventually boil down to generating revenue, safeguarding revenue (e.g. from a competitor) or reducing costs. The authors don't really discuss, never mind suggest how to measure, the relationship between software development and added value. 

This is probably because the framework they are describing isn't about measuring *productivity*. The focus, especially at the individual level, is on measuring *activity*. The unspoken and un-evidenced assumption being that activity has a strong correlation with added value and therefore productivity.

The authors introduce a matrix of measurements at three different levels. System, team and individual. The closer they get to the individual level, the more the metrics chosen measure activity. Many of these activity metrics are identified as being sourced from the [SPACE framework](https://queue.acm.org/detail.cfm?id=3454124). While the authors of the SPACE framework do talk about measuring activity, they apply lots of caveats which are not really reflected in the McKinsey article. Some highlights from their paper I enjoyed, for example:

{{< admonition type=quote title="" open=true >}}

Myth: Productivity is all about developer activity

{{< /admonition >}}

{{< admonition type=quote title="" open=true >}}

For example, if you're already measuring commits (an activity measure), don't simply add the number of pull requests and coding time to your metrics dashboard, as these are both activity metrics.

{{< /admonition >}}

{{< admonition type=quote title="" open=true >}}

Teams and organizations should be cognizant of developer privacy and report only anonymized, aggregate results at the team or group level. 

{{< /admonition >}}

It seems the McKinsey authors are contradicting, or at least misunderstanding, the work they are trying to build upon. Then there are some of the specific metrics they chose to include in their framework. 

Let's look at story points completed as an example. Leaving aside the debate about story points being a useful practice (the person who [probably invented them says not](https://ronjeffries.com/articles/019-01ff/story-points/Index.html)), they are an abstract relative measure of effort, complexity and risk. Not a unit of value. It's perfectly possible to efficiently churn out hundreds of story points of code that nobody wants or needs. They are also very subjective and, used in any kind of performance metric, trivially easy for suitably incentivized developers to game.

The other one I want to talk about is McKinsey's secret sauce - contribution analysis. The authors do not go into detail on how this is performed stating only:

{{< admonition type=quote title="" open=true >}}

Assessing contributions by individuals to a team’s backlog (starting with data from backlog management tools such as Jira, and normalizing data using a proprietary algorithm to account for nuances)

{{< /admonition >}}


Ah Jira. The safe space of the mediocre manager. There is a temptation to treat the contents of a work tracking database like Jira as the source of truth about a team's work. In fact, tools like Jira contain an out-of-date, eventually consistent, reductive *abstraction* of the work that is actually happening. Nothing contained in Jira, in any state, necessarily represents added value. It's a representation of an output at best, and it's a reductive view of what a team does. 

How is collaboration accounted for? An insightful contribution to a group discussion around a whiteboard? Taking the time to help a newer team member with a complex area of the problem domain or technology? 

It's a recipe for incentivizing selfishness, and it doesn't  measure what the article claims to measure. 

And speaking of reductive...

### Reductionism

Metrics are inherently reductive. They simplify a complex, multifaceted system like software development into a handful of numbers by eliminating context, nuance and detail. Context, nuance and detail that is significant to the effectiveness and outcomes of that system, the system that will now be managed as if they doesn't exist.

The act of measuring changes behaviour (often one of the goals of measuring things in the first place) but in a complex system it is difficult to predict how it will change. When context and nuance is removed the likelihood of creating perverse incentives that undermine the actual desired business outcomes is high.

The authors (partially) acknowledge and lament this.

{{< admonition type=quote title="" open=true >}}

Other functions can be measured reasonably well, some even with just a single metric; whereas in software development, the link between inputs and outputs is considerably less clear. 

{{< /admonition >}}

{{< admonition type=quote title="" open=true >}}

Unlike a function such as sales, where a system-level metric of dollars earned or deals closed could be used to measure the work of both teams and individuals, software development is collaborative in a distinctive way that requires different lenses.

{{< /admonition >}}

But is it even true that the productivity of the other business functions the article mentions can be measured well with just a single reductive metric, without jeopardizing broader outcomes? Incentivising a metric like dollars earned never has toxic side effects or leads to suboptimal business outcomes? Sales that could be closed now pushed into the next reporting period to juice the numbers? Aggressive discounts offered to incentivize closing a deal slightly earlier than it otherwise would? Hospital pass projects sold to meet a sales target?

Even if we accept this is possible in other fields of endeavour, the article doesn't explain how it's set of outcome and activity metrics solve this problem in software development. Because it doesn't. It can't. It doesn't even demonstrate an understanding of what important details might be rendered invisible by implementing their framework.

### More Contradictions 

The article presents a model where software development activities are split into inner and outer loops. The inner loop consisting of three activities: code, test and build. The outer loop is presumably intended to encompass everything else but specifically identifies meetings, integrate, security and compliance and deploy at scale. 

The authors make a couple of statements about this model:

{{< admonition type=quote title="" open=true >}}

The less time spent on less fulfilling, outer loop activities, the better.

{{< /admonition >}}

{{< admonition type=quote title="" open=true >}}

For example, one company that had previously completed a successful agile transformation learned that its developers, instead of coding, were spending too much time on low-value-added tasks such as provisioning infrastructure, running manual unit tests, and managing test data.

{{< /admonition >}}

Ignoring the '[successful agile transformation remark'](https://i.kym-cdn.com/entries/icons/original/000/023/021/e02e5ffb5f980cd8262cf7f0ae00a4a9_press-x-to-doubt-memes-memesuper-la-noire-doubt-meme_419-238.png), I have some notes.

The implication is that activities in their outer loop are low value add. This contradicts the research lead by [Dr Nicole Forsgren](https://nicolefv.com/) that the authors appropriate, without citing her by name, when they endorse the DORA metrics. Those metrics are designed to measure the effectiveness of [24 capabilities](https://firefinch.io/accelerate-24-key-capabilities/) shared by [high-performing organizations](https://www.goodreads.com/book/show/35747076-accelerate). 

Two of those capabilities are *test data management* and *shift left on security*. McKinsey themselves seem to endorse shifting left on security, directly involving developers in security considerations early in the development process, in [a podcast.](https://www.mckinsey.com/industries/technology-media-and-telecommunications/our-insights/giving-developers-a-leading-role-in-cybersecurity)

This new, even more reductive model the authors are trying to sell us directly contradicts the excellent work they are building upon. It's also worth a note that teamwork seems to get stuck in the low value add category with *meetings*, continuing a theme of devaluing or ignoring collaboration. 

### No Detail, No Evidence

The pitch depends on two measures we are told very little about: the aforementioned contribution analysis and a bespoke metric called 'Developer Velocity Index'. While there is another article on McKinsey's site that goes into a bit more [detail on DVI](https://www.mckinsey.com/industries/technology-media-and-telecommunications/our-insights/developer-velocity-how-software-excellence-fuels-business-performance) there is no explanation of the methodology used to measure these metrics. 

There is no actionable advice. No way to attempt to reproduce and verify their work. The authors claim that their framework has been implemented at 'nearly 20' organizations and has resulted in some quantifiable benefits, but with no methodology or data published and a relatively small sample size there is no real evidence of efficacy. 

## From the Ashes

It's not great but there are some useful things that can be salvaged from the article itself and the resulting discussion. 

### Why Does Anyone Care About Measuring Productivity Anyway?

As mentioned earlier, this is a perceptive question, and it's worth revisiting. You should ask this question, or a variation of it, whenever the topic of attempting to measure productivity is arises. What question are you trying to answer where having a productivity metric would help? 

Questions about productivity are often good examples of an [XY problem.](https://en.wikipedia.org/wiki/XY_problem) Try to redirect the conversation to the desired goal because there are almost certainly better options available to achieve it than implementing a reductive, destructive framework as described in the article. 

### Happiness, Satisfaction and Psychological Safety

Intuitive, 'common sense' conclusions about the world are often simplistic and incorrect. In this case though, the idea that happy people who find satisfaction in their work are more productive is well supported by evidence collected over a long time by psychologists[^3]. 

One of the conclusions of Google's [Project Aristotle](https://psychsafety.co.uk/googles-project-aristotle/) study was that high performing teams consistently exhibit high levels of psychological safety - the shared belief that all team members are respected and free to express their thoughts, ideas an opinions without fear of negative consequences. 

[Team health checks](https://engineering.atspotify.com/2014/09/squad-health-check-model/) are a useful practice to help you try and quantify these properties. I won't recommend a specific format or set of questions (there are lots of examples out there you can look at to inform what you want to do) but I will make some general recommendations:

* Identify a small, core set of questions that focus on these properties and ask them to every team. This will give you the ability to make meaningful comparisons.
* It's good to let teams tailor their own health checks to gather additional data they find useful.
* But don't let the check become so big, or do it so frequently, it becomes a box ticking chore. Genuine reflection and thought over a small number of areas is better than a large number of data points skimmed through by a bored or disengaged team. I would want a *maximum* of a dozen data points or questions.
* Try not to chop and change questions too regularly. You want to be able to identify trends.
* Give people the option of providing qualitative as well as quantitative feedback.  

This is a self assessment, so it is subjective and depends upon team members trusting that they won't face negative consequences for providing honest feedback. If you are trying to build trust in what has historically been a low-trust environment you can try making the feedback anonymous. It shouldn't need to be said but if you do this make sure it *actually* is anonymous. 

This isn't performance data, nor a set of metrics to be incentivized. You are looking for patterns and trends that allow you to ask more insightful questions so you can provide better support.

### Outcomes

Probably the only sound piece of actionable advice in the article is the use of the 4 key metrics identified in [Accelerate.](https://www.goodreads.com/book/show/35747076-accelerate) These are:

* Deployment Frequency - How often an organization successfully releases to production. 
* Lead Time For Changes - The amount of elapsed time from starting a change to releasing it to production.
* Change Failure Rate - The percentage of production releases that cause a failure in production.
* Time To Restore Service - The time taken to recover from a failure in production.

These can be measured at the organization level and (if you have aligned your teams and deployable components correctly) at the team level. These metrics correlate with strong continuous delivery performance. They can give you the ability to compare with other organizations and, to an extent, compare teams within your organization. The best use of them is to observe patterns and trends, and use those observations to ask better questions about the [underlying capabilities](https://firefinch.io/accelerate-24-key-capabilities/) and how they can be improved.

Note that you are measuring outcomes here, not business impact or added value. It's perfectly possible to very effectively deliver software that no-one wants or needs, has no business impact and adds no value. Speak of the devil.

### Business Impact and Added Value

Two ways of basically saying the same thing. Software doesn't exist in a vacuum. Organizations create software in order to affect some kind of change in the wider world. That change, or impact, is usually achieved by delivering something that provides value to someone. 

At it's simplest the equation looks something like this:

<br/>

$$
\begin{equation}
\text{Team} + \text{Work} = \text{Output} \implies \text{Impact}
\end{equation}
$$

You need to work with whoever sets the strategy for your organization to figure out how to measure that impact, which will vary by the type of business and the strategy employed. For instance, a start-up may care about capturing market share or large numbers of users to pump a future valuation. An established business with a successful product may care more about maintaining revenue and high margins from it's existing customer base.

There are [tools](https://en.wikipedia.org/wiki/Value-stream_mapping) you can [use](https://teamtopologies.com/key-concepts) to be able to draw an imaginary line from your teams, through their output and outcomes, to a quantifiable business impact.

## TL;DR
 
The failure to acknowledge the importance of added value and business impact is troubling. In my opinion, the focus on reductive measurements of activity - especially at the individual level - will harm the organization by encouraging the selfish pursuit of perverse incentives over more holistic business goals. 

It's also disingenuous of McKinsey to make claims without backing them up with rigorous evidence. Two quotes from the article stand out as significant:

{{< admonition type=quote title="" open=true >}}

The long-held belief by many in tech is that it’s not possible to do it correctly - and that, in any case, only trained engineers are knowledgeable enough to assess the performance of their peers.

{{< /admonition >}}

{{< admonition type=quote title="" open=true >}}

Learn the basics. All C-suite leaders who are not engineers or who have been in management for a long time will need a primer on the software development process and how it is evolving.

{{< /admonition >}}

The authors have identified a genuine need and created a narrative that appeals to the biases of their target audience. By suggesting that non-experts can easily grasp the essentials of software development and that expert opinions are flawed, they simplify the complexities of software development into a model that feels intuitively correct to those outside the field. Activity measurements can be applied universally with little variation, and are easy to implement. This lends itself well to a high margin, repeatable consultancy package.

When there is demand, especially from those with money to spend, businesses will step in with a product or service to meet that demand. [Some will prioritize sales](https://www.telegraph.co.uk/finance/newsbysector/retailandconsumer/6658864/Boots-we-sell-homeopathic-remedies-because-they-sell-not-because-they-work.html) over efficacy.

As a piece of research on how to measure development productivity and as a piece of actionable advice, the article is mostly a failure. As marketing for a low-cost, high-margin, easily deliverable consultancy package, though? It looks like it worked pretty well.

[^1]: Assuming unlimited demand for widgets.

[^2]: If you are less inclined to assume good faith, [Ron Jeffries](https://ronjeffries.com/articles/-y023/dev-prod/) covers that perspective well.

[^3]: So well supported that it was hard to find a good summary to link to. Just google it. You will have reading for days.