---
layout: post
title: Analytics in Visual Studio Team Services
---
Getting actionable metrics out of Visual Studio Team Services can be painful. I've published a [couple](https://christopher-bimson.github.io/2017-03-26-visual-studio-team-services-cfd-dashboard-widget/) of [hacks](https://github.com/christopher-bimson/VstsMetrics) to make it slightly easier to track lead time and cycle time on a VSTS project, but it's still a chore.

The good news is Microsoft have released a public preview of an [Analytics extension for VSTS](https://marketplace.visualstudio.com/items?itemName=ms.vss-analytics#overview) that provides these metrics and handy dashboard widgets to make them transparent. 

Take a look:

{: .center}
![Cumulative Flow Diagram](/img/analytics/cfd.png)

{: .center}
![Cycle Time](/img/analytics/cycle-time.png)

{: .center}
![Lead Time](/img/analytics/lead-time.png)

The data behind the widgets is available via an [OData](https://docs.microsoft.com/en-us/vsts/report/extend-analytics/) interface and integration with [Power BI](https://docs.microsoft.com/en-us/vsts/report/powerbi/). I haven't had the chance to try this out yet. The documentation is currently a little light, but I'm hoping this will make it easier to consume this data in more detailed analysis.

Analytics is free while it is in public preview, so it's worth giving it a spin.

