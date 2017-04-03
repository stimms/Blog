---
layout: post
title: Quick A/B Testing in ASP.net MVC - Part 3 Storing clicks
authorId: simon_timms
date: 2014-01-20
---

<div style="background-color:#f8f8f8;">This blog is the third in a series about how to implement A/B testing easily in ASP.net MVC by using Redis

1. [Having 2 pages](http://blog.simontimms.com/2014/01/06/quick-ab-testing-in-asp-net-mvc-part-1-having-2-pages/ "Quick A/B Testing in ASP.net MVC â€“ Part 1 Using 2Pages")
2. Choosing your groups
3. Storing user clicks
4. Seeing your data

</div>So far we've seen how to select pages semi randomly and how to have two different views. In this post we'll figure out a way of storing the click through statistics on the server. For the server I've chosen [Redis](http://redis.io/). My reasoning was largely around how simple programming against Redis is and also that it has a built in primitive which allows for incrementing safely. At the time of writing Redis doesn't have good support for clustering (wait until version 3!) but there is support for master-slave configurations. Weirdly our data access pattern here is more writes than reads, so having a multi-master setup might be better.For most purposes a single master should be enough to support A/B record keeping as Redis is very fast and 99% of websites aren't going to be taxing for it. If you really are getting more hits than a Redis server can handle then perhaps reduce the percentage of people who end up doing the testing.

Redis is likely better run on a Linux box than Windows. Although there is a Windows version it was developed largely by Microsoft and it not officially supported. At the current time there are no plans to merge the Windows support back into mainline. Still the Windows version will work just fine for development purposes. I installed Redis using ChocolateyNuget

cinst redis

You can do that or download the package and do it manually.

There are a couple of .net clients available for Redis but I went with one called Booksleeve which was developed by the Stackoverflow team. This can be installed from nuget

install-package booksleeve

Now we need to write to Redis as needed. I created a simple interface to handle the logging.

<script src='https://gist.github.com/stimms/8392037.js'></script>

LogInitialVisit is called whenever somebody visits and A/B page and LogSuccess is used when the user has performed the activity we want. The initial visit call can be hooked into the BaseController we created in part 1.

<script src='https://gist.github.com/stimms/8392788.js'></script>

The actual Redis implementation of IABLogger looks like:

<script src='https://gist.github.com/stimms/8392976.js'></script>

In a real implementation just using localhost for the Redis server is likely not what you want. What isn't apparent here is that the call to Redis is performed asynchronously inside Booksleeve so this should have very little impact on the speed of page rendering.

The initial visit counter should be correctly updated. Now we need a way to update success counter. There are a couple of approaches to this but I think the best is simply to provide an end point for the views to call out for when â€œsuccessâ€ is achieved. Success can be visiting a link or just pausing on the page for a period of time; an endpoint which accepts POSTs is sufficiently flexible to handle all eventualities.

The controller looks like

<script src='https://gist.github.com/stimms/8394319.js'></script>

I'm a big fan to TypeScript so I wrote my client side code in TypeScript

<script src='https://gist.github.com/stimms/8394285.js'></script>

The ready function at the end hooks up links which have appropriate data- attributes. This does mean that a post is sent to our server before sending people on to their final destination. Google does more or less the same thing with search results.

If we check the results in Redis we can see that appropriate keys have been created and values set.

redis 127.0.0.1:6379> keys About* 1) "About.AboutA.success" 2) "About.AboutB.success" 3) "About.AboutA" 4) "About.AboutB" redis 127.0.0.1:6379> get About.AboutA "8" redis 127.0.0.1:6379> get About.AboutA.success "1"

In the next part we'll look at how to retrieve and analyse the statistics we've gathered.



