---
layout: post
title: Selector Selection
authorId: simon_timms
date: 2013-06-17
---

In code review last week the topic of efficient CSS selectors in your JavaScript came up. I have always been a fan of using as simple selectors as possible. Typically I use a class name on the elements I'm selecting. Others on the team are fans of using more complex selectors such as

body > div > div:first > span:first > p

I hate this. I think that it is confusing for a human to figure out what's being selected there without hunting through the DOM. I also think it isincrediblybrittle. All it takes is adding one element somewhere in that chain and your selectors stop working. With dynamic pages that hierarchy may well be open to change even during the life of the page. I think this example is particularly bad because it descends right from the root of the body. Even something simple like adding a notification box is going to break this.

My argument was not wellreceived. The counter argument was that any changes to the page would require a change to the JavaScript anyway so it isn't a big deal. Humm"¦ okay, different approach.

Well I didn't have the stats on speed of selectors at the time but today I found some. The benchmark somebody build at [jsperf.com](http://jsperf.com/jquery-selector-benchmark/22)is fantastic for my purposes.

<div class="wp-caption aligncenter" id="attachment_2864" style="width: 760px">[![Benchmark!  Way better than a benchmatt. ](http://stimms.files.wordpress.com/2013/06/screen-shot-2013-06-17-at-7-54-09-pm.jpg?w=750)](http://stimms.files.wordpress.com/2013/06/screen-shot-2013-06-17-at-7-54-09-pm.jpg)Benchmark! Way better than a benchmatt.

</div>Ah ha! Looks like my method of selecting using just a class is way more efficient than parent child selectors (51% slower vs 80% slower than optimal). However my method is still quite a bit slower than having a context and using find. I was a bit surprised by that because I expected that with the commonality of selecting with a class would have been optimized to theumpteenthdegree. I was even more shocked to see that specifying the tag and then a class wasn't faster than just selecting a class.

I think the take away here is to use classes over the confusing mess above(sorry, coworkers) and that having an id at least on a parent container allows for more efficient selection. I'm also not convinced that these aren't micro-optimizations which should be ignored but as the rules above can be used in place of each other you might as well use the more optimal version.



