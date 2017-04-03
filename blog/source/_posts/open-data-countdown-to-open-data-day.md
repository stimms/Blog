---
layout: post
title: Open Data -  Countdown to Open Data Day
authorId: simon_timms
date: 2013-02-19
---

Only a few more days to go before Open Data Day is upon us. For each of the next few days I'm going to look at a set of data from one of the levels of government and try to get some utility out of it. Some of the data sets which come out of thegovernmenthave noconceivableuse to me. But that's the glory of open data, somebody else will find these datasets to be more useful than a turbo-charged bread slicer.

Today I'm looking at some of the data the provincial government is making available through [The Office of Statistics and Information](https://osi.alberta.ca/). This office seems to be about theequivalentof StatsCan for Alberta. They have published a large number of data set which have been divided into categories of interest such as "Science and Technology", "Agriculture", "Construction". Drilling into the data sets typically gets you a graph of the data and the data used to generate the graph. For instance looking into Alberta Health statistics about infant mortality gets you to [this page](https://osi.alberta.ca/osi-content/Pages/Catalogue.aspx?ipid=943&GOAViewMode=factsheet).

The Office of Statistics and Information, which I'll call OSI for the sake of my fingers, seems to have totally missed the point of OpenData. They have presented data as well as interpretation of the data as OpenData. This is a cardinal sin, in my mind. OpenData is not about giving people data you've already massaged in a CSV file. It is about giving people the raw, source data so that they can draw their own conclusions. Basically give people the tools to think by themselves, don't do the thinking for them.

The source data they give doesn't provide any advantage over the graph, in fact it is probably worse. What should have been given here is an anonymized list of all the births and deaths of infants in Alberta broken down by date and by hospital. From that I can gather all sorts of other interesting data such as

- <span style="line-height:13px;">Percentage of deaths at each hospital</span>
- Month of the year when there are the most births(always fun for making jokes about February 14th + 9 months)
- The relative frequency of deaths in the winter compared with those in the summer

For this particular data set we see reference to zones. Whatdelineatesthese zones? I went on a quest to find out and eventually came across a map at the [Alberta Health](http://www.albertahealthservices.ca/ahs-map-ahs-zones.pdf) page. The map is, of course, PDF. Without this map I would never have known that Tofield isn't in the Edmonton zone while the much more distantKapasiwin is. The reason for this is likely lost in the mists of government bureaucracy. So this brings me to complaint number two: don't lock data intoartificialcontainers. I should not have to go hunting around to find the definition of zones, they should either be linked off the data page or, better, just not used. Cities are a pretty good container for data of this sort, if the original data had been set up for Calgary,Edmonton, Banff,"¦ then its meaning would have been far more apparent.

Anyway I promised I would do something with the data. I'm so annoyed by the OSI that this is just going to be a small demonstration. I took the numbers from the data set above and put them in to the map from which I painstakingly removed all the city names.

<div class="wp-caption aligncenter" id="attachment_2340" style="width: 619px">[![Infant mortality in Alberta](http://stimms.files.wordpress.com/2013/02/alberta1.jpg)](http://stimms.files.wordpress.com/2013/02/alberta1.jpg)Infant mortality in Alberta

</div>Obviously there are a million factors which determine infant mortality but all things being equal you should have your babies in Calgary. You should have them here anyway because Calgary has the highest concentration of awesome in theprovince. Proof? I live here.



