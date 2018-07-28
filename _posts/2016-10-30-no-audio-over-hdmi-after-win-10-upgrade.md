---
layout: single
title: No Audio Over HDMI After Windows 10 Upgrade
tags: [Yak Shaving, Windows 10]
redirect_from: "/2016-10-30-no-audio-over-hdmi-after-win-10-upgrade/"
---
Sometime after my otherwise successful upgrade to Windows 10 I connected my laptop to a Samsung TV like I had done a million times before and was irked to discover that the sound output was no longer working. 

As I expected, the google consensus was that I needed some driver updates. After a wasting a good 30 minutes  installing and reinstalling various drivers I stumbled across the solution by accident. 

If you have a similar problem, try checking the default playback format of your device before wading into the driver quagmire, like so:

*  Find your Monitor/TV/whatever in the [Playback Devices](https://blogs.technet.microsoft.com/mediaq/2015/07/30/windows-10-audio-troubleshooting-tips/) window (see below) and view it's properties.

   ![Playback Devices window](/img/audio/enable_tele_noise_1.png){: .align-center}

*  On the Advanced Tab check the Default Format. For me, Windows had chosen to set this to '24bit, 48000 Hz (Studio Quality)'. Lowering this value a bit (see below) restored the audio.

   ![Lower Quality Default Format](/img/audio/enable_tele_noise_2.png){: .align-center}