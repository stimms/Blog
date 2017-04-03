---
layout: post
title: Browser Performance Counters
authorId: simon_timms
date: 2013-06-19
---

I was reading a blog post the other day on dealing with memory leaks in gmail, I'vecompletelylost the link now but it put me onto a lot ofinterestingclient side tools of which I was onlytangentiallyaware. Readers of this blog will know that I'm on this constant push towards shifting functionality from the server to the client. My push is just part of the great cycle of architecture.

<div class="wp-caption aligncenter" id="attachment_2867" style="width: 752px">[![Hooray! It's another one of Simon's MSPaint graphs](http://stimms.files.wordpress.com/2013/06/logic-location.jpg)](http://stimms.files.wordpress.com/2013/06/logic-location.jpg)Hooray! It's another one of Simon's MS Paint graphs

</div>In another 10 years we'll start moving all our functionality back to the server as a new generation discovers the power of centralizing your logic. It is a funny old world.

Anyway this post is about the information available to you within chrome's performance namespace. If you open up any page in chrome and hit F12 you'll get the developer tools. In there grab the console and type "˜performance'. The object returned has a number of properties, largely these properties are filled with counters for memory and load times.

There are a lot of counters in there so let's take it one by one. The first one is performance.memory. This contains a MemoryInfo object which in turn contains 3 counters. jsHeapSizeLimit is the maximum sizeavailableto the heap, usedJSHeapSize is the amount of memory currently used and totalJsHeapSize is the total currently reserved heap space including segments not currently occupied. These counters can be used to find pages which use a lot of memory or find memory leaks in long living single page applications.

The next set of counters are timing records. Typically you would do timings on the server side but the client side is really far moreinteresting as it gives a true reflection of your user's experience. The timing stuff is pretty complete and is specified in the [W3"²s Navigation Timing specification](https://dvcs.w3.org/hg/webperf/raw-file/tip/specs/NavigationTiming/Overview.html). They have this awesome picture to give you an idea of when the timings are taken

<div class="wp-caption aligncenter" id="attachment_2868" style="width: 760px">[![Timeline](http://stimms.files.wordpress.com/2013/06/timing-overview.png?w=750)](http://stimms.files.wordpress.com/2013/06/timing-overview.png)Timeline

</div>The timing information is only available in the client but you can easily wrap it up and send it back to your server. This performance information gives you a great way to gather as much information as possible about what's happening for your users. You can find slow DNS lookups, inefficient edge caching, complicated dom events. I'm really excited to see what ideas I can come up with for this information.

These counters are available on all the modern web browsers and also Internet Explorer(ooooohhh, BURN).



