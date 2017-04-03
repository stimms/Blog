---
layout: post
title: Mobile is Getting Cool
authorId: simon_timms
date: 2013-03-27
---

I'm way behind the times on mobile development. It is one of those things I've been meaning to get into but just haven't had the cycles. There is a mobile app on my horizon so I've been looking a bit into how I would go about it. From what I can see there are currently two good options for building mobile applications at the moment. Ishouldstop and explain mycriteriabefore somebody shives on me for jumping to conclusions.

First is that it needs to run on as many devices as possible. Realistically an Android tablet and iPad would be sufficient(sorry, Surface). This is obviously a pretty common use case these days. The tablet market has been pretty much owned by iPad but recently Android has been making inroads. I have a Nexus 7 which is really a fantastic device. Of course I'm comparing it with a first generation iPad so things on the Apple side might have come along some way.

Second is that it needs to be easy to develop. I've never been very impressed with Objective C and developing for the Dalvik VM using Java is grim. I'm really sad about where Java's going as of late. There are Java security flaws all over the web as of late and the autoupdate mechanism install highly questionable software along with the updates. I still think that the JVM is a good platform, I just think that Java has lost its way.

With these criteria in mind I think the two great options for developing for mobile are Mono and Phone Gap. Mono started off as a port of the .net framework to Linux but has now really got legs. You can now write the majority of your application in C# and have it run on Android or iOS. Phone Gap allows you to write your application in HTML5 but still have access to most of the underlying device's hardware. For the things that don't have an API yet you can write bindings, but they have to be donenativelyand, obviously, they need to be written for every platform.

In my mind both Phone Gap and Mono provide powerful programming tools and models for phone development. There arecertainlysome things for which you might want to write a native app but I bet they make up less than 5% of the apps on phones. If I had to develop an app today without being given any opportunity for further research I think I would probably jump into Phone Gap. I'm really excited about all the good work going on in the HTML/JavaScript space at the moment. I think the ecosystem for HTML based phone applications will greatlybenefitfrom the larger world of HTML and JavaScript development.

The mobile space is getting really interesting, far more interesting than it was even two years ago. I bet it will be even more exciting in two years.



