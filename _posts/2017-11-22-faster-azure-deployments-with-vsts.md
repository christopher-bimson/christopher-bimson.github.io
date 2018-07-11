---
layout: single
title: Faster Azure Deployments
tags: [Visual Studio Team Services, Azure]
---
Fixing a failing release definition in Visual Studio Team Services can be a chore. To make things a little easier you might be inclined to leave debug output enabled, so you have the maximum amount of data available when the inevitable breakages happen.

This can have a surprising performance cost if your pipeline makes use of in-line Azure PowerShell tasks. I observed one task spend 57 seconds outputting debug information prior to starting execution, which took about 5 seconds.

Setting the ```System.Debug``` release variable to ```false``` shaved about 7 minutes off the run time of my team's release pipeline, and we haven't missed the debug output yet.

