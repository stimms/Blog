---
layout: post
title: Excel is XML -  When EPPlus Doesn't Support You
authorId: simon_timms
date: 2013-03-22
---

If your application needs to write out Excel files then your best option is to make use of the near magical [EPPlus](http://epplus.codeplex.com/). I've written about [EPPlus before](http://blog.simontimms.com/2013/03/07/creating-databars-in-epplus/ "Creating DataBars inEPPlus")and today's post actually stems from that one. See the default databars which EPPlus puts in are a little limited. The specific issue the customer had was that when a value of 0 was entered the databar still showed a little bit. If you manually add databars through Excel the problem doesn't exist. I was curious about what options were missing in the excel file created by EPPlus which existed in the Excel created file. Could I duplicate them in EPPlus?

Excel is a pretty old application dating back to when I was just a wee lad. As you might expect over time the file format became more and more complicated as new features were added and hacks put in (ask me about how dates work in excel). In Office 2007 Microsoft threw out their old file format and moved to a new, XML based, file format. They did it for all the major office applications: Word, Excel and Powerpoint. Access is still a disaster, but you should be using LightSwitch, right? It is a far better file format than the old one even though it is XML.

Excel file are actually a collection of XML files all zipped up with a standard zip algorithm. That means that you can just rename your .xlsx file to .zip and you can open it up with your fully registered version of WinZip. Inside you'll find a bunch of files but the one we're intersted in is sheet1.xml.

<div class="wp-caption aligncenter" id="attachment_2485" style="width: 310px">[![Excel Contents](http://stimms.files.wordpress.com/2013/03/screen-shot-2013-03-21-at-11-16-24-pm.jpg?w=300)](http://stimms.files.wordpress.com/2013/03/screen-shot-2013-03-21-at-11-16-24-pm.jpg)Excel Contents

</div>I put in some simple databars and opened up this file's XML. I won't include the full contents here as it is pretty long but you can look at it in [a gist](<script src='https://gist.github.com/stimms/5219131.js'></script>). Comparing this with what EPPlus generated the difference became apparent: the whole extLst section was missing. My understanding is that these extList and ext sections are used to provide extensions to the file format. So while the conditional formatting section existed in EPPlus' version it didn't have the extensions and it was the extensions which provided the styling I wanted.

Fortunately there is a solution inside EPPlus. When defining the worksheet there is a property called WorksheetXML which contains the full XML document which will be exported when the excel package is saved. I tied into this XML using the Linq2XML API and injected the appropriate extLst information and the line in the conditional formatting to link to it. It took me a little while to get everything set up the way it needed to be but that was mainly because of my mental block around XML namespaces.

It certainly isn't a pretty way to manipulate Excel documents but if you need some functionality which hasn't been bound by EPPlus yet then it's a pretty good solution. Being able to open an existing Excel file and make small changes then compare it with the previous version gives an easy path to reverse engineer the file format.



