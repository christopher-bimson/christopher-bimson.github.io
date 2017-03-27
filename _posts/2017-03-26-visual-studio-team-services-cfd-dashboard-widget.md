---
layout: post
title: A Cumulative Flow Diagram For The Visual Studio Team Services Dashboard
subtitle: Less than five minutes. Zero lines of code. No warranty. 
---
The dashboard in Visual Studio Team Services isn't a replacement for [big visible charts](http://ronjeffries.com/xprog/articles/bigvisiblecharts/) 
but it provides a handy little library of [widgets](https://www.visualstudio.com/en-us/docs/report/widget-catalog) that can be used to display a lot of useful information at a glance. 

The widget catalog contains a burndown chart for teams that work in fixed length iterations, but while VSTS has a cumulative flow diagram for teams who take a more Kanban-esque approach, it is not yet available in widget form. An area graph created from a work item query may make an effective substitute or, if you are really dedicated, you could probably create a widget using the [API.](https://www.visualstudio.com/en-us/docs/integrate/extensions/develop/add-dashboard-widget) But who has time for that?       

## Hack

Turns out it is possible to display the VSTS cumulative flow diagram on the dashboard with a little bit of URL hacking. Here's how:

1. View the URL of the VSTS cumulative flow diagram in a browser. It looks something like this:
   
   ```
   https://{your-account}.visualstudio.com/{guid}/_api/_teamChart/CumulativeFlow?chartOptions={HTML encoded gibberish}
   ```

1. The HTML encoded gibberish is actually a JSON document that, when decoded, looks a bit like this:
   
   ```
   {
     "Width":1386,
     "Height":686,
     "ShowDetails":true,
     "Title":""
   }
   ```

1. Add an embedded web page widget to your VSTS dashboard. A size of 4x2 seems to work well.

1. Tweak the JSON document in step 2 to get the size of chart that you want (600x300 seemed OK to my eyes with the 4x2 widget) 
   then HTML encode it again.

1. Set the URL of the embedded web page widget to the URL of the chart in step 1, replacing the value of the `chartOptions` query string
   parameter with your newly encoded JSON document.

1. Sit back and admire your new cumulative flow diagram widget.

{: .center}
![Example Cumulative Flow Diagram](/img/vsts-cfd/dashboard-cropped.png)