---
layout: single
title: Forecasting
tags: [Forecaster, No Estimates]
redirect_from: "/2017-04-19-forecaster/"
---

This [post by Neil Killick](https://neilkillick.wordpress.com/2017/01/09/what-would-my-noestimates-book-look-like/) has a good summary of the dysfunctional behaviors that seem to be permanently wedded to estimation in software development. It's a great post, and you should read it all, but I want to focus on two points that are relevant to something I've been mucking about with recently.

> Not incorporating appropriate levels of uncertainty into an estimate (e.g. giving a precise date when a set of scope will be delivered, saying a precise set of scope will be delivered on a fixed date, not calling out risks/assumptions/issues/dependencies as part of the estimate, etc.)

and

> Due to the complex nature of product development, teams should only estimate individual backlog items and forecast how much they can do in a short timespan (e.g. 2-week sprint/iteration)
For batches of items beyond this short timeframe (e.g. releases), empirical forecasting using “yesterday’s weather” should be used to answer “how long” or “when” questions, not asking developers.

## Monte Carlo Forecasting

An easy way to help address both of Neil's points is to create forecasts using the [monte carlo method.](https://en.wikipedia.org/wiki/Monte_Carlo_method) These forecasts are based on historical performance, can provide a range of possible outcomes with a likelihood for each and should not require any additional time from your development team. 

Here is a worked example[^1] to explain how to make one:

1. The first input to the forecast is historic performance data for your team, measured in some units of work per time period. In a Scrum team this could be velocity in story points per iteration. In a team using a Kanban process it might be weekly throughput. For the purposes of creating a forecast, the specific units used are not important.

   To keep this example simple we'll assume a Scrum team with a velocity measured in story points per iteration.

   | Iteration     | Velocity      |
   | ------------- | --------------|
   | 1             | 6             |
   | 2             | 8             |
   | 3             | 5             |
   | 4             | 13            |
   | 5             | 7             |

2. The next input is the number of time periods into the future that you want to forecast. These are the same time units used in the historic data described in step 1. 

   We'll assume we want to forecast our next release, which will be in 8 iterations time.      

3. The last input is the number of trials. Think of each each trial as a potential 'future timeline' for your team. As a gross simplification, the more trials used, the more confident you can be in the forecast[^2].

   Because this is an example, we'll pick an unreasonably low number of trials. Say 5.

4. We populate each time period in each trial by randomly sampling the historic data from step 1. This will result in a data set that looks a bit like this:

   | Iteration | Trial 1 | Trial 2 | Trial 3 | Trial 4 | Trial 5 |
   | -------- -| ------- | ------- | ------- | ------- | ------- |
   | 1         |     13  |     13  |      5  |       8 |       5 |
   | 2         |     8   |     8   |     13  |       5 |       5 |
   | 3         |     8   |     5   |      6  |       8 |       6 |
   | 4         |     13  |     7   |      7  |       5 |       5 |
   | 5         |     8   |     5   |      6  |       8 |       5 |
   | 6         |     5   |     5   |     13  |       5 |      13 |
   | 7         |    13   |     8   |      6  |       5 |       8 |
   | 8         |     8   |     8   |      5  |       5 |      13 |
   | Total     |    76   |    59   |     61  |      49 |      60 |

5. We can summarize the trials in step 4 by expressing the likelihood of achieving a certain number of story points as the percentage of the total number of trials where that number of
story points was achieved.
  
   | Story Points Achieved | Likelihood |
   | --------------------  | ---------  |
   | 40                    |      100%  |
   | 50                    |       80%  |
   | 60                    |       60%  |
   | 70                    |       20%  |
   | 80                    |        0%  |

   To continue with the crap sci-fi metaphor, think of this as the percentage of future timelines where our customer's increasingly wild dreams come to pass.
 
## Explicit Uncertainty

Probably the most common method used to create empirical forecasts is to multiply the average (mean) of a team's historic performance by some number of time periods, and then express that as a single figure. If the team in the example above did this they would have come up with a forecast of roughly 62 story points in 8 iterations. 

I can guarantee that anyone exposed to that unqualified 62 story point figure will automatically attribute a likelihood of 100%[^3] to it with no further comment. Unfortunately the actual likelihood is more like 50%. All kinds of important business decisions are now going to be made based on odds no better than a coin toss. Oops.

This is where the monte carlo forecast adds value. It produces a range of possible outcomes with an explicit level of uncertainty for each, better enabling 'the business' to trade off risk and reward when making decisions.

## Some Code

It's unfashionable to do maths with your own brain, so I wrote some code to create these simple forecasts. You can find it on [GitHub.](https://github.com/christopher-bimson/Forecaster) This is how to use it to recreate the example above:

```shell
forecast.exe --historicThroughput 6 8 5 13 7 --periodsToForecast 8
```

And this is what the output will look like:

```shell
|   Forecast Throughput|            Likelihood|
|       At Least 40 WUs|               100.00%|
|       At Least 50 WUs|                96.41%|
|       At Least 60 WUs|                61.59%|
|       At Least 70 WUs|                18.47%|
|       At Least 80 WUs|                 2.18%|
|       At Least 90 WUs|                 0.10%|
```

- - - 
[^1]: The generous people at Focused Objective have some more detailed examples and exercises available on their [GitHub.](http://bit.ly/SimResources)

[^2]: It's a bit beyond the scope of this post, but there are methods you can use to determine the exact number of trials you need to run to achieve a particular confidence value in the outcome. 

[^3]: They probably mentally substituted the word 'forecast' for the word 'commitment' too.