---
layout: post
title: An Easy Cumulative Flow Diagram Widget For Your Visual Studio Team System Dashboard
subtitle: Less than five minutes. Zero lines of code. No warranty. 
---
If you are a Visual Studio Team Services user, you may have noticed the rollout of many new features aimed at making [Kanban a first class citizen
of the VSTS and TFS world.](https://www.visualstudio.com/en-us/docs/work/kanban/kanban-basics)

If you are familar with Kanban, you will know that the cumulative flow diagram is one of the prinipal tools used to visualize the progress of work as it moves 
through a system. [VSTS has a cumulative flow diagram](https://www.visualstudio.com/en-us/docs/report/guidance/cumulative-flow) and it's getting more useful 
with each release. It's also exactly the kind of [big visible chart](http://ronjeffries.com/xprog/articles/bigvisiblecharts/) that us Agile types are fond of. Unfortunately, at the time of writing, there is no build-in widget to add
a continuous flow diagram to your team's [VSTS dashboard.](https://www.visualstudio.com/en-us/docs/report/dashboards) You could use the [API](https://www.visualstudio.com/en-us/docs/integrate/extensions/develop/add-dashboard-widget) 
to create one. Probably. But who has time for that?

## Works For Me

There may not be cumulative flow diagram widget, but there is one that will 
[embed a web page](https://www.visualstudio.com/en-us/docs/report/widget-catalog) in your dashboard using an iframe. 