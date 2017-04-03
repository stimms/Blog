---
layout: post
title: Speeding up page loading â€“ Part 3
authorId: simon_timms
date: 2013-12-16
---

Welcome to part 3 of my series on speeding up pages loading. The [first ](http://blog.simontimms.com/2013/12/04/speeding-up-page-loading-part-1/ "Speeding up page loading â€“ part1") [two](http://blog.simontimms.com/2013/12/09/speeding-up-page-loading-part-2/ "Speeding up page loading â€“ part2")parts were pretty generic in nature but this one is going to focus in a little bit more on ASP.net. I think the ideas are still communicable to other platforms but the tooling I'm going to use is ASP.net only.

Most of the applications you're going to write will be backed by a relational database. As websites become more complicated the complexity of queries tends to increase but also the number of queries increases. In .net a lot of the complexity of queries is hidden by abstraction layers like Entity framework and NHibernate. Being able to visualize the queries behind a piece of code which operates on an abstract collection is very difficult. Lazy loading of objects and the nefarious n+1 problem can cause even simple looking pages to become query rich nightmares.

Specialized tooling is typically needed to examine the queries running behind the scenes. If you're working with the default Microsoft stack(ASP.net + Entity Framework + SQL Server) there are a couple of tools at which you should look. The first is Hibernating Rhino's [EFProf](http://www.hibernatingrhinos.com/products/efprof). This tool hooks into Entity Framework and provides profiling information about the queries being run. What's better is that it is able to examine the patterns of queries and highlight anti-patterns.

The second tool is the absolutely stunning looking [Glimpse](http://getglimpse.com/). Glimpse is basically a server side version of the F12/FireBug/Developer Tools which exists in modern browsers. It profiles not just the SQL but also which methods on the server are taking a long time. It is an absolute revolution in web development as far as I'm concerned. Its SQL analysis is not as powerful as EFProf but you can still gain a great deal of insight from looking at the generated SQL.

We'll be making use of Glimpse in this post and the next. There is already a fine [installation guide](http://getglimpse.com/Help/Getting-started) available so I won't go into details about how to do that. In the application I was profiling I found that the dashboard page, a rather complex page, was performing over 300 queries. It was a total disaster of query optimization and was riddled with lazy loading issues and repeated queries. Now a sensible developer would have had Glimpse installed right from the beginning and would have been watching it as they developed the site to ensure nothing was getting out of hand. Obviously I don't fall into that category.

<div class="wp-caption aligncenter" id="attachment_3107" style="width: 760px">[![A lot of queries](http://stimms.files.wordpress.com/2013/12/screen-shot-2013-12-14-at-1-54-32-pm.jpg?w=750)](http://stimms.files.wordpress.com/2013/12/screen-shot-2013-12-14-at-1-54-32-pm.jpg)A lot of queries

</div>So the first step is to reduce the number of queries which are run. I started by looking for places where there were a lot of similar queries returning a single record. This pattern is indicative of some property of items in a collection being lazily loaded. There were certainly some of those and they're an easy fix. All that is needed is to add an include to the query.

Thus the query which looked like

<script src='https://gist.github.com/stimms/7966866.js'></script>

became

<script src='https://gist.github.com/stimms/7966891.js'></script>

In another part of the page load for this page the users collection was iterated over as was the permission collection. This caused a large number of follow on queries. After eliminating these queries I moved onto the next problem.

If you notice in the screenshot above the number of returned records is listed. In a couple of places I was pulling in a large collection of data just to get its count. It is much more efficient to have this work done by the SQL server. This saves on transferring a lot of unneeded data to the web serving tier. I rewrote the queries to do a better job of using the SQL server.

These optimizations helped in removing a great number of the queries on the dashboard. The number fell from over 300 to about 30 and the queries which were run were far more efficient. Now this is still a pretty large number of queries so I took a closer look at the data being displayed on the dashboard.

Much of the data was very slow changing data. For instance the name of the company and the project. These are unlikely to change after creation and if they do probably only once. Even some of the summary information which summarized by week would not see significant change over the course of an hour. Slow changing data is a prime candidate for caching.

ASP.net MVC offers a very easy method of doing caching of entire pages or just parts of them. I split the single large monolithic page into a number of components each one of which was a partial view. For instance this section calls to 3 partial views

<script src='https://gist.github.com/stimms/7967317.js'></script>

As each graph changes at a different rate the caching properties of the graph are different. The methods for the partial is annotate with an output cache directive. In this case we cache the output for 3600 seconds or an hour.

<script src='https://gist.github.com/stimms/7967374.js'></script>

Sometimes caching the entire output from a view is more than you want. For instance one of the queries which was being commonly run was to get the name of the company from the Id. This is called in a number of places not of them in a view. Fortunately the caching mechanisms for .net are available outside of the views

<script src='https://gist.github.com/stimms/7967701.js'></script>

One thing to remember is that the default caching mechanism in ASP.net is a local cache. So if you have more than one server running your site(and who doesn't these days?) then the cache value will not be shared. If shared cache is needed then you'll have to look to an external service such as the [Azure cache](http://www.windowsazure.com/en-us/manage/services/cache/net/how-to-cache-service/) or [ElastiCache](https://aws.amazon.com/elasticache/)or perhaps a memcache or Redis server in your own data center.

On my website these optimizations were quite significant. They reduced page generation time from about 3 seconds to 200ms.An optimization I didn't try as it isn't available in EF5 is to use asynchronous queries. When I get around to move the application to EF6 I may give that a shot.





