---
layout: post
title: Building Your Source Controlled Access Database
authorId: simon_timms
date: 2013-04-17
---

Last week I published an oddly popular post about [version controlling access databases](http://blog.simontimms.com/2013/04/12/so-you-want-to-version-you-access-database/ "So You Want to Version You AccessDatabase"). Could be that other people are also responsible for maintaining an access database? Let's pause for a moment and sigh collectively.

<<SIGH>>

If you read this blog with any frequency(hi, mom!) then you'll know that I'm big on producing automatic builds. I'm not the only one, it is even part of the [Joel test](http://www.joelonsoftware.com/articles/fog0000000043.html). How can we get access to build from the source files automatically? Unsurprisingly it turns out it sucks.

Because the source files are separated from the compiled database something needs to assemble the files into the accdb file. In an ideal world the assembly would be done by a command line build tool. No such tool exists. I tried to create one but I ended up deep in a COM hole attempting to call the source control bindings in Access without having to open up the UI. Turns out that this COM stuff, which largelydisappearedbefore I started programming on Windows, is kind of hard. I backed away from thatapproachand instead I made use of a UI automation library called White.

White is an open source project which I believe came out of Thoughworks back in the day. Since then it has been picked up a group of people known as [TestStack](http://teststack.github.io/pages/white.html). There was a flurry of activity when they first started maintaining the tool but that's died of since. I'll do a more in detailed post on White and how to use it at a later date.

Unfortunately, I can't release everything I did to get Access to build the sourcecontrolledversion of my database as myemployerisn't big on releasing things through open source. I think I can get away with some general hints, though.

The first thing is to launch Access without any flags which, once you've included White.Core looks like

var application = Application.Launch(@"c:Program Files (x86)Microsoft OfficeOffice14MSACCESS.EXE");

Then you basically just drive the UI as if it were you at the keyboard. Selecting a ribbon tab requires using the Window.Tabs collection and firing a Click event on the appropriate tab, in this case Source Control. Then it is just a question of selecting Create from Team Foundation and driving the rest of the UI.

With this approach I could easily generate the required accdb file. I haven't got there yet but I'm going to tie the building application into TFS as a build step so that full packages can be generated.



