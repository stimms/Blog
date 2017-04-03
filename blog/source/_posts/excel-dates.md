---
layout: post
title: Excel Dates
authorId: simon_timms
date: 2013-05-23
---

I love hearing stories of how things came to be in computing. One of my favorite stories is about date in Microsoft Excel. I stumbled across this story while writing some spreadsheet import code.

The first thing you need to know is that Excel stores date as a number of days since January 0<sup>th</sup> 1900. So a date with a value of 1 is January 1<sup>st</sup> 1900. You can see this by selecting a date field and changing the format to general. You'll get a number like 41408 which is the 14<sup>th</sup> of May 2013. So that's pretty simple except for leap years. As we all know in a leap year (every 4 years) there is an extra day in the year. However just adding a day every 4 yearsisn'tquite right. Every 100 years we skip a leap year. Every 400 years we have an extra leap year. That's why the year 2000 was a leap year even though it is divisible by 100 (because it is also divisible by 400). Keep these leap year rules in mind, we're coming back to them later.

Back in the mid-1980s a little company named Microsoft was trying to build a new spreadsheet program. Spreadsheets were the killer applications of the 1980s, heck they probably still are the most used applications on business computers. VisiCalc was the first consumer spreadsheet and its influence can be seen to this day. The A1 style of addressing cells waspopularizedby VisiCalc, although you can probably trace the origins further back than that. VisiCalc was released in 1979 but by 1983 it had beensupplantedby the vastly superior Lotus 1-2-3. Microsoft had already tried to compete with VisiCalc and Lotus 1-2-3 with a program called Multiplan. It did not go well.

<div class="wp-caption aligncenter" id="attachment_2764" style="width: 650px">[![Lotus 1-2-3. I totally remember this.](http://stimms.files.wordpress.com/2013/05/lotus-123-30-dos.jpg)](http://stimms.files.wordpress.com/2013/05/lotus-123-30-dos.jpg)Lotus 1-2-3. I totally remember this.

</div>To be competitive with Lotus 1-2-3 Microsoft needed something new. Excel was the answer to this. To compete with the dominant player, Lotus,they needed to be able to open up Lotus 1-2-3 files, edit them and save them back again as Lotus 1-2-3 files.One of the problems they encountered enabling that was that there was a bug in Lotus 1-2-3 in how it handled dates. Lotus 1-2-3 thought that the year 1900 was a leap year so it had 366 days in that year. To maintain compatibility Microsoft included that bug in Excel. Even to this day you'll find you can enter February the 29<sup>th</sup> 1900 despite the fact that day never existed.

The bug in Lotus 1-2-3 has survived long after the death of Lotus. It's been 30 years but we still see the influence of one minor mistake. Amazing.

All this means that when you're converting an excel date to another date format you actually have to take the number of days Excel tells you, subtract 2 (one for the leap year and one for the fact there is no 0<sup>th</sup> of January) then add that to 1900-01-01.



