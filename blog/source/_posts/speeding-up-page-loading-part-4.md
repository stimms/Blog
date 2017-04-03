---
layout: post
title: Speeding up page loading â€“ Part 4
authorId: simon_timms
date: 2013-12-23
---

In the [first](http://blog.simontimms.com/2013/12/04/speeding-up-page-loading-part-1/ "Speeding up page loading â€“ part1") [three](http://blog.simontimms.com/2013/12/09/speeding-up-page-loading-part-2/ "Speeding up page loading â€“ part2") [parts](http://blog.simontimms.com/2013/12/16/speeding-up-page-loading-part-3/ "Speeding up page loading â€“ Part3") of this series we looked at JavaScript and CSS optimizations, image reduction and query reduction. In this part we'll look at some options around optimizing the actual queries. I should warn you straight up that I am not a DBA. I'm only going to suggest the simplest of things, if you need to do some actual query optimizations then please consult with somebody who really understands this stuff like [Mike DeFehr](http://mikedefehr.com/)or [Markus Winand](http://use-the-index-luke.com/).

The number of different queries run on a site is probably smaller than you think. Certainly most pages on your site are, after the optimizations from part 3, going to be running only a couple of queries with any frequency. Glimpse will let you know what the queries are but, more importantly, it will show you how long each query takes to execute. 

Without knowing an awful lot about the structure of your database and network it is hard to come up with a number for how long a query should take. Ideally you want queries which take well under 100ms as keeping page load times is [important](http://www.strangeloopnetworks.com/assets/images/visualizing_web_performance_poster.jpg). People hate waiting for stuff to load, which is, I guess, the whole point of this series.

Optimizing queries is a tricky business but one trick is to take the query and paste it into SQL Management Studio. Once you have it in there click on the actual execution plan button. This will show you the execution plan which is the strategy the database believes is the optimal route to run the query.

[![Screen Shot 2013-12-19 at 10.08.56 PM](http://stimms.files.wordpress.com/2013/12/screen-shot-2013-12-19-at-10-08-56-pm.jpg)](http://stimms.files.wordpress.com/2013/12/screen-shot-2013-12-19-at-10-08-56-pm.jpg)



If you're very lucky the query analyser will suggest a new index to add to your database. If not then you'll have to drill down into the actual query plan. In there you want to see a lot of

<div class="wp-caption aligncenter" id="attachment_3120" style="width: 316px">[![Index seek](http://stimms.files.wordpress.com/2013/12/screen-shot-2013-12-19-at-10-23-05-pm.jpg)](http://stimms.files.wordpress.com/2013/12/screen-shot-2013-12-19-at-10-23-05-pm.jpg)Index seek

</div>Index seeks and index scans are far more efficient than table seeks and scans. Frequently table scans can be avoided by adding an index to the table. I blogged a while back about how to [optimize queries with indexes](http://blog.simontimms.com/2013/05/16/sql-indexes/ "SQLIndexes"). That article has some suggestions about specific steps you can take. One which I don't think I talk about in that article is to reduce the number of columns returned. Unfortunately EF doesn't have support for selectively returning columns or lazily loading columns. If that level of query tuning appeals to you then you may wish to swap out EF for something like [Dapper](https://github.com/SamSaffron/dapper-dot-net) or [Massive](https://github.com/robconery/massive)which are much lower level.

If you happen to be fortunate enough to have a copy of Visual Studio <span style="color:#ff0000;">ULTIMATE <span style="color:#000000;">(or, as the local Ruby user group lead calls it: Ultra-Professional-Premium-Service-Pack-Two-Release-2013) then there is another option I forgot to mention in part 3. IntelliTrace will record the query text run by EF. I still think that Glimpse and EFProf are better options as they are more focused tools. However Glimpse does sometimes get a bit confused by single page applications and EFProf costs money so IntelliTrace will work.</span></span>



