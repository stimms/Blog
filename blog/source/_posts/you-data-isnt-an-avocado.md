---
layout: post
title: You Data isn't an Avocado
authorId: simon_timms
date: 2013-01-24
---

At my local supermarket you can buyavocadostwo different ways: either you get the individual avocados or you buy a bag of five avocados. The individual avocados are never ripe; they are as hard as agovernessin a 19th century English novel. The bags of five are usually borderline overripe. This means that if you want to make anything with avocado the day you're shopping you have to be prepared to eat five avocados in a single sitting. Now I like guacamoles as much as the next guy but five avocados is a hell of a lot.

<div class="wp-caption aligncenter" id="attachment_2157" style="width: 610px">[![I will have the enchilada platter with two tacos and no guacamoles](http://stimms.files.wordpress.com/2013/01/super-troopers-broken-lizard-8107609-853-480.jpg?w=600)](http://stimms.files.wordpress.com/2013/01/super-troopers-broken-lizard-8107609-853-480.jpg)I will have the enchilada platter with two tacos and no guacamoles

</div>It is difficult to get just the right number and ripeness of avocados for my liking. Usually I have to buy a bunch of bulk avocados and wait rotate them out of the fridge one or two at a time so they're not all ripe at the same time. A lot of application development is trying to get people data which is ripe. But you know what? Sometimes you can buy the data earlier and wait for it to ripen.

*I don't get it.*

Yeah it isn't the best analogy but I just ate 5 avocados worth of guacamole so I'm not all that coherent. What I'm getting at is that even though people tell you theyabsolutelyneed the latest data to make their decisions they don't. I work in the oil and gas market and the majority of the people with whom I deal are the tree hating, printing reports type. If they are printing reports then they're never going to have the latest data. The state of the system can change radically even from the time they hit print to the time they pick the paper up on the printer.

It is a bit of a change of mindset for developers to appreciate the fact that they don't always have to query the database for information. Caching isn't a new concept but typically it has only been used for expensive operations. What I suggest is that you should flip your mindset around caching from â€œcache when expensiveâ€ to â€œquery when staleâ€. Cache everything.

There are some great tools and techniques out there for doing this at an application level. One of my favorite is to make use of an aspect weaver like Postsharp to [wrap data queries](http://www.sharpcrafters.com/blog/post/solid-caching.aspx). This allows you to write your code as normal but simply annotate the repository methods with an attribute which will cause the weaver to intercept the method invocation and pull the answer from the cache instead of from source.

The onlyobstacleto caching is knowing when to invalidate the cache and cause a requery. That is where I would suggest you spend your business analysis time. How frequently does data change? How much importance should be placed on having fresh data?If you happen to have an event log from an active system then it is pretty easy to calculate how frequently data changes.

Caching has a significant speed advantage and will allow your application to scale further with the same database. Databases are generally the key scalability bottlenecks in most systems and being able to delay difficult and expensive database rearchitecting or replacement is almost certain to pay off.



