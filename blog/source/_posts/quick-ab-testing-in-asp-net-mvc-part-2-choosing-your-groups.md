---
layout: post
title: Quick A/B Testing in ASP.net MVC - Part 2 Choosing Your Groups
authorId: simon_timms
date: 2014-01-14
---

<div style="background-color:#f8f8f8;">This blog is the second in a series about how to implement A/B testing easily in ASP.net MVC by using Redis1. [Having 2 pages](http://blog.simontimms.com/2014/01/06/quick-ab-testing-in-asp-net-mvc-part-1-having-2-pages/ "Quick A/B Testing in ASP.net MVC "“ Part 1 Using 2Pages")
2. Choosing your groups
3. Storing user clicks
4. Seeing your data

</div>In part one of this series we looked at a simple way to selectively render pages. However we did not take a look at how to select which users end up in each of the test groups. There is almost certainly some literature on how to select test groups. Unfortunately my attempts at investigation met with a wall of spam from marketing sites. Well we'll just jolly well make up our own approach.

Time for an adventure!

[![OLYMPUS DIGITAL CAMERA](http://stimms.files.wordpress.com/2014/01/adventure.jpg)](http://stimms.files.wordpress.com/2014/01/adventure.jpg)

My initial approach was to randomly assign requests to be either in A or B. However this approach isn't ideal as it means that the same user visiting the page twice in a row may encounter different content. That seems like the sort of thing which would confuse many a user and it would irritate me as a user. It is best to not irritate our users so we need a better approach.

We can easily write a cookie to the user's browser so they get the same page on each load. That is rather grim. We like nothing more than avoiding cookies when we can. They are inefficient and, depending on how they are implemented, a pain in a cluster. A better solution is to pick a piece of information which is readily available and use it as a hash seed. I think the two best pieces are the username and failing that (for unauthenticated users) the IP address. So long as we end up having an approximately even distribution into the A and B buckets we're set.

We've been talking about having only two buckets: A and B. There is no actual dependence on there being but two buckets. Any number of buckets is fine but the complexity does tend to tick up a bit with more buckets. I have also read some suggestions that you might not wish to set up your testing to unevenly weight visits to your page. If you have a page which already works quite well you can direct, say, 90% of your users to that page. The remaining 10% of users become the testing group. In this way the majority of users get a page which has already been deemed to be good. The math to see if your new page is better is pretty simple, a little bit of Bayes and you're set.

We'll take a pretty basic approach.

<script src='https://gist.github.com/stimms/8381557.js'></script>

Here we differentiate between authenticated and unauthenticated user, each one has a different strategy. The authenticated users use their user name to select a group. User names are pretty static so we should be fine hashing them and using them as a hash seed.

<script src='https://gist.github.com/stimms/8381618.js'></script>

For unauthenticated users we use the IP address as a hash value. In some cases this will fall down due to people being behind a router but it is sufficient for our purposes.

In the next post we'll log the clicks into Redis.



