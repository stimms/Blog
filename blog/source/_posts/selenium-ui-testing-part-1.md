---
layout: post
title: Selenium - UI Testing - Part 1
authorId: simon_timms
date: 2010-12-15
---

I had a bug to fix earlier this week which was really just a UI bug. The details aren't all that important but it was something like a field in a popup div wasn't being properly updated. This felt a lot like something which could use not just a test in the back end to ensure the correct data was being passed back but also a test in the front end to make sure it was being displayed. As our front end is all webby, as most front ends tend to be these days. Years ago I did some work with [Mercury QuickTest](http://en.wikipedia.org/wiki/HP_QuickTest_Professional) as it was then called(I believe it is now owned by HP which makes it pure evil, just like HP-UX). I looked at a couple of tools for automating the browser and decided on [selenium](http://seleniumhq.org/) partially because it appeared to be the most developed and partially because I couldn't refuse a tool with an atomic mass of 78.96.

This is the part where I warn you that I really have no idea what I'm doing, I'm no selenium expert and I'll probably change my mind about how to best implement it. I'll post updates as I change my mind.

I started by installing the selenium IDE and, on another computer, the remote control. The IDE is just a plugin for firefox while the RC is a java based tool which happily runs on windows. I opened up a port for it in the windows firewallâ€¦ oh who am I kidding I turned the damn thing off.

[![](http://stimms.files.wordpress.com/2010/12/seleniumide1.png?w=300)](http://stimms.files.wordpress.com/2010/12/seleniumide1.png)I recorded a number of actions using the IDE and added a number of verifications(you can add a check by right clicking on an element and selecting one of the verify options in there). I don't consider these to be unit tests since they tend to cross over multiple pages. What I'm creating are work flow tests or behavioral tests. These could be used as part of BDD in conjunction with a tool such as [Cucumber](http://cukes.info/) or [SpecFlow](http://www.specflow.org/). As such I don't really care how long these tests take to run, I don't envision them being run as part of our continuous integration process but rather as part of hourly builds. If things really get out of hand(and they won't our application isn't that big) then the tests can be distributed over a number of machines and even run in the cloud on some Amazon instances.

In the Selenium IDE I exported the test to C# so they could be run in our normal build process. Unfortunately the default export template export nunit tests and our project makes use of the visual studio test suite. There are no technical reasons for not having both testing frameworks but I don't want to burden the other developers too much with a variety of frameworks. So my job for tomorrow is to explore alternatives to the standard export. I am also not crazy about having to maintain the tests in C#, it requires that a developer be involved in testing changes when it should really be easy enough for a business person to specify the behaviour. I have some ideas about running the transformations from Selenium file format(which is an html table) into C# using either T4 or the transformation code extracted from the IDE running in a server side javascript framework.

I'll let you know what I come up with. Chances are it will be crazy, because sensible isn't all that fun.

**Useful Blogs**

- [Design of Selenium tests for Asp.net](http://code.google.com/p/design-of-selenium-tests-for-asp-net/)A blog with some specific suggestions about how to work with selenium and asp.net. I'm not in 100% agreement with his ideas but I might change my mind later.
- [Adam Goucher's blog](http://adam.goucher.ca/) â€“ Adam is a hell of a nice guy who has been helping me out with Selenium on the twitter.



