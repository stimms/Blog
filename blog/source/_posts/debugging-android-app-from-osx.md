---
layout: post
title: Debugging Android App from OSX
authorId: simon_timms
date: 2013-02-13
---

I just got a Nexus 7 for my birthday and thought I would try deploying to it. I really have no idea about building for Android so there were a couple of stumbling blocks for me I thought I would write down for future reference.

1. The Nexus 7 and, I understand, Android devices in general after 4.2 don't have a development option on the menu. You have to go to settings > about and then tap on the build number 7 times to enable it. Hilarious. I'm delighted we basically have a Konami code for getting to options.

2. By default the USB debugging is turned off. You need to turn that on from the developer menu to get it to actually be found by the android SDK. You can check your devices by running

adb devices

adb is found in the platform-tools directory of the SDK.



