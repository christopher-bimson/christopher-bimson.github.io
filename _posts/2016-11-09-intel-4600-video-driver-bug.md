---
layout: single
title: Four Free 1080p Monitors Courtesy of Intel!
subtitle: Fixing a bad Intel 4600 graphics driver on Windows 10.
tags: [Yak Shaving, Windows 10]
redirect_from: "/2016-11-09-intel-4600-video-driver-bug/"
---
I was lucky enough to win four brand new monitors in the latest round of Windows updates.

![Windows 10 Display Settings](/img/video-driver/all-the-screens.png){: .align-center}

After much messing about I discovered that this was apparently due to a bug in the [latest version](https://downloadcenter.intel.com/download/26229/Intel-Graphics-Driver-for-Windows-10-15-40-4th-Gen-) of the Intel graphics driver for Windows 10 that was recently pushed out over Windows Update.

Previous versions of the driver are available at the [Intel Download Centre](https://downloadcenter.intel.com), but at the time of writing downloads appear to be throttled to the point they will not complete and there do not appear to be any official mirrors.

Somewhat ironically, Windows Update came to the rescue. A searchable history of all updates is  available at the [Windows Update Catalog](http://www.catalog.update.microsoft.com/Home.aspx). I chose to roll back to version [20.19.15.4444](http://download.windowsupdate.com/d/msdownload/update/driver/drvs/2016/06/200024121_7313824213686619ff3705954006a19add581307.cab) of the driver and installed it manually like so:

*  Unzip the .cab file containing the older display driver.
*  Open **[Device Manager](https://support.microsoft.com/en-us/instantanswers/005a1acb-776e-4320-b9f2-3a2302a320da/open-device-manager)**, find the Intel 4600 under **Display adapters** and double click on it.
*  Select the **Driver** tab and click **Update Driver...**
*  Select **Browse my computer for driver software**.
*  Select **Let me pick from a list of device drivers on my computer**.
*  Click **Have Disk...**
*  In the **Install From Disk** dialog box click **Browse...**
*  Navigate to the folder where you unzipped the .cab file, select the ```igdlh64.inf``` file and click **Open**.
*  Click the **OK** button in the **Install From Disk** dialog box.
*  The driver should now be selected in the wizard. Click **Next**.

The phantom screens are now banished.
