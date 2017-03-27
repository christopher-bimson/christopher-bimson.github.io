---
layout: post
title: A Cumulative Flow Diagram For The Visual Studio Team System Dashboard
subtitle: Less than five minutes. Zero lines of code. No warranty. 
---
The dashboard in Visual Studio Team Services isn't a replacement for [big visible charts](http://ronjeffries.com/xprog/articles/bigvisiblecharts/) 
but it has a handy little library of [widgets](https://www.visualstudio.com/en-us/docs/report/widget-catalog) that can display a lot of useful information at a glance. 
There is a burndown chart for teams that work in fixed length iterations and, while VSTS does have a cumulative flow diagram for teams who take a more Kanban-esque approach, it is not available in widget form yet.

You might be able to simulate the cumulative flow diagram for your teams workflow by creating an area graph from a work item query. 
If you are really dedicated you could probably create a widget yourself using the [API](https://www.visualstudio.com/en-us/docs/integrate/extensions/develop/add-dashboard-widget) - but who has time for that?       

## Hack

Turns out it is possible to display the VSTS cumulative flow diagram on the dashboard with a little bit of URL hacking. Here's how:

1. View the URI of the VSTS cumulative flow diagram in a browser. It looks something like this:
   
   `https://{your-account}.visualstudio.com/{some guid}/_api/_teamChart/CumulativeFlow?chartOptions={some HTML encoded gibberish}` 

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
1. Set the URL of the embedded web page widget to the chart URI in step one, replacing the value of the `chartOptions` query string
   parameter with your newly encoded JSON document.
1. Sit back and admire your new cumulative flow diagram widget.

![Example Cumulative Flow Diagram](/img/vsts-cfd/dashboard-cropped.png)