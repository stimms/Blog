---
layout: post
title: How to Write a Bug Report
authorId: simon_timms
date: 2013-03-20
---

I could not believe it when I checked and found that I don't have a post on how to write a bug report. Over the years I've seen a lot of bug reports. Of course most of them aren't real bugs as that would imply that the code I write isn't perfect. Aridiculousproposition. The bug reports are usually substandard in some way.

The purpose of a bug report is to explain to the developers or support group exactly what went wrong. To do this I like to see bugs which tell a story:

> I was attempting to load the add user form from the main menu but when I clicked on "Add User" I was taken to the user list screen. I expected to be taken to a screen where I could add the user.

This story contains the key elements which a developer would need to reproduce the bug. Obviously this is a very simple scenario and one in which it is apparent what the issue is. As the scenarios get more complicated the information needed from the user increases.Fortunatelymuch of this information can be gained from examining logs instead of pestering the user with questions like "What server were you on?" and "At what time did the failure occur?".

Make no mistake writing bug reports is a pain and for every issue you get a report of there are likely to be a dozen users who just give up. Frustratedusers aren't what you want. Making reporting as simple as possible for users should be a key part of your user interaction strategy. To streamline this process I usually suggest using a tool like [jing](http://www.techsmith.com/jing.html). This allows users to quickly record their screen so they don't even need to type in a bug report.

This still requires that users let you know when they have problems. I just stumbled across [MouseFlow](http://mouseflow.com/)which is the actualization of an idea that I had some time back for tracking user interactions in the browser. Traditional web analytics let you know what users are doing on your site, where they are clicking and what they're looking at. MouseFlow allows you to look at how users are acting on the page. As the web evolves more of the user interactions are moving within a page using Ajax or even single page applications. These are more difficult to profile than traditional websites. Some interactions don't need to make server trips so the user's activities are lost. MouseFlow captures these interactions and gives you some great tools for analyzing them.

I haven't used MouseFlow but it looks amazing. When I do get around to trying it out then I'll post back with my experiences.



