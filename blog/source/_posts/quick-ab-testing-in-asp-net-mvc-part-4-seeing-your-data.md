---
layout: post
title: Quick A/B Testing in ASP.net MVC â€“ Part 4 Seeing your data
authorId: simon_timms
date: 2014-02-10
---

<div style="background-color:#f8f8f8;">This blog is the fourth and final in a series about how to implement A/B testing easily in ASP.net MVC by using Redis

1. [Having 2 pages](http://blog.simontimms.com/2014/01/06/quick-ab-testing-in-asp-net-mvc-part-1-having-2-pages/ "Quick A/B Testing in ASP.net MVC â€“ Part 1 Using 2Pages")
2. [Choosing your groups](http://blog.simontimms.com/2014/01/14/quick-ab-testing-in-asp-net-mvc-part-2-choosing-your-groups/ "Quick A/B Testing in ASP.net MVC â€“ Part 2 Choosing YourGroups")
3. [Storing user clicks](http://blog.simontimms.com/2014/01/20/quick-ab-testing-in-asp-net-mvc-part-3-storing-clicks/ "Quick A/B Testing in ASP.net MVC â€“ Part 3 Storingclicks")
4. Seeing your data

</div>Now that we've been running our tests for some time and have a good set of data built up it is time to retrieve and examine the data.

We've been following a pretty convention based approach so far: relying on key names to group our campaigns. This pays off in the reporting interface. The first thing we do is set up a page which contains a list of all the campaigns we're running.

<script src='https://gist.github.com/stimms/8873789.js'></script>

This code retrieves all the keys from redis and passes them off to the view. The assumption here is that the entire redis instance is dedicated to running AB testing. If that isn't the case then the A/B testing data should be namespaced and the find operation can take a prefix instead of just *. I should warn that listing all the keys in Redis is a relatively slow operation. It is not recommended for typical applications. I am confident the number on this site will remain small so I'll let it slide for now. A better approach is likely to store the keys in a Redis set.

In the view we'll just make a quick list out of the passed in keys, filtering them into groups.

<script src='https://gist.github.com/stimms/8873846.js'></script>

For our example this gives us

[![Screen Shot 2014-02-07 at 4.09.43 PM](http://stimms.files.wordpress.com/2014/02/screen-shot-2014-02-07-at-4-09-43-pm.jpg?w=300)](http://stimms.files.wordpress.com/2014/02/screen-shot-2014-02-07-at-4-09-43-pm.jpg)For the details page things get slightly more complicated.

<script src='https://gist.github.com/stimms/8874000.js'></script>

Here we basically look for each of the subkeys for the passed in key and then get the total hits and successes. If your subkeys are named consistently with A, B, C then this code can be much cleaner and, in fact, the key query to Redis can be avoided.

Finally in the view we simply print out all of the keys and throw a quick progress bar in each row to allow for people to rapidly see which option is the best.

[![Screen Shot 2014-02-07 at 4.12.54 PM](http://stimms.files.wordpress.com/2014/02/screen-shot-2014-02-07-at-4-12-54-pm.jpg?w=750)](http://stimms.files.wordpress.com/2014/02/screen-shot-2014-02-07-at-4-12-54-pm.jpg)The code for this entire project is up on github at[https://github.com/stimms/RedisAB](https://github.com/stimms/RedisAB)



