---
layout: post
title: Open Data -  Countdown to Open Data Day 3
authorId: simon_timms
date: 2013-02-21
---

This is day 3 of my countdown to Open Data Day. That sneaky Open Data Day is slithering and sneaking up on us like a greased snake on an ice rink. So far we've looked at data from the provincial government and the city. That leaves us just one level of government: the federal government. I actually found that the feds had the best collection of data. Their site [data.gc.ca](http://data.gc.ca)has a huge number of data sets. What's more is that the government has announced that it will be adopting the same open government system which has been developed jointly by India and the US: [http://www.opengovtplatform.org/](http://www.opengovtplatform.org/).

One of the really interesting things you can do with open data is to merge multiple data sets from different sources and pull out conclusions nobody has ever looked at before. That's what I'll attempt to do here.

Data.gc.ca has an amazing number of data sets available so if you're like me and you're just browsing for something fun to play with then you're in for a bit of a challenge. I eventually found a couple of data sets related to farming in Canada which looked like they could be fun. The first was a set of data about farm incomes and net worths between 2001 and 2010. The second was as collection of data about yields of various crops in the same time frame.

I started off in excel summarizing and linking these data sets. I was interested to see if there was acorrelationbetween high grain yields per hector and an increase in farm revenue. This would be a reasonable assumption as getting more grain per hector should allow you to sell more and earn more money. Using the power of Excel I merged and cut up data sets to get this table:

<table border="0" cellpadding="0" cellspacing="0" width="338"><col width="65"></col><col width="78"></col><col width="86"></col><col width="109"></col><tbody><tr><td height="15" width="65"></td><td width="78">Farm Revenue</td><td width="86">Yield Per Hector</td><td width="109">Production in tonnes</td></tr><tr><td align="right" height="15">2001</td><td align="right">183267</td><td align="right">2200</td><td align="right">5864900</td></tr><tr><td align="right" height="15">2002</td><td align="right">211191</td><td align="right">1900</td><td align="right">3522400</td></tr><tr><td align="right" height="15">2003</td><td align="right">194331</td><td align="right">2600</td><td align="right">6429600</td></tr><tr><td align="right" height="15">2004</td><td align="right">238055</td><td align="right">3100</td><td align="right">7571400</td></tr><tr><td align="right" height="15">2005</td><td align="right">218350</td><td align="right">3200</td><td align="right">8371400</td></tr><tr><td align="right" height="15">2006</td><td align="right">262838</td><td align="right">2900</td><td align="right">7503400</td></tr><tr><td align="right" height="15">2007</td><td align="right">300918</td><td align="right">2600</td><td align="right">6076100</td></tr><tr><td align="right" height="15">2008</td><td align="right">381597</td><td align="right">3200</td><td align="right">8736200</td></tr><tr><td align="right" height="15">2009</td><td align="right">381250</td><td align="right">2800</td><td align="right">7440700</td></tr><tr><td align="right" height="15">2010</td><td align="right">356636</td><td align="right">3200</td><td align="right">8201300</td></tr><tr><td align="right" height="15">2011</td><td align="right">480056</td><td align="right">3300</td><td align="right">8839600</td></tr></tbody></table>Looking at this it isn't apparent if there is a link. We need a graph!

I threw it up against d3.js and produced some code which was very similar to my previous bar chart example in [HTML 5 Data Visualizations "“ Part 5 "“ D3.js](http://blog.simontimms.com/2013/01/25/html-5-data-visualizations-part-5-d3-js/ "HTML 5 Data Visualizations "“ Part 5 "“D3.js")

<div class="wp-caption aligncenter" id="attachment_2356" style="width: 760px">[![Grain yields in blue, farm revenues in orange](http://stimms.files.wordpress.com/2013/02/screen-shot-2013-02-21-at-2-45-09-pm.jpg)](http://stimms.files.wordpress.com/2013/02/screen-shot-2013-02-21-at-2-45-09-pm.jpg)Grain yields in blue, farm revenues in oranges

</div>I didn't bother with any scales because it isimmediatelyapparent that there does not seem to be any correlation. Huh. I would have thought the opposite.

You can see a live demo and the code over at[http://bl.ocks.org/stimms/5008627](http://bl.ocks.org/stimms/5008627)



