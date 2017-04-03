---
layout: post
title: When to Unit Test
authorId: simon_timms
date: 2013-03-18
---

A few days back there was a big debate about when to do unit testing and how much should be tested. A bunch of the programmers I really admire like Greg Young, Jimmy Bogadt and Uncle Bob Martin weighed in. The debate was about what you would expect: Uncle Bob was prosthelytizing his belief that testing is key and that the quest should be for 100% code coverage.

> New Blog. The Start-Up Trap. [http://t.co/341M4wNDh2](http://t.co/341M4wNDh2)
> 
> â€” Uncle Bob Martin (@unclebobmartin) [March 5, 2013](https://twitter.com/unclebobmartin/status/308977353621659648)

<script async="" charset="utf-8" src="//platform.twitter.com/widgets.js"></script>

On the other side of the debate the more pragmatic Greg young and Jimmy Bogard were suggesting that blindly testing everything is anaiveapproach. One should pick and choose the code to test. How much code should be tested? Well [it depends](http://lostechies.com/jimmybogard/2013/03/12/elaborating-on-it-depends/). I've been in the same boat as Jimmy, testing nothing, testing everything and testing somethingin-between. What's the right mixture? I have no idea.

To me it comes down to: what is of value to your business. What's theconsequenceto failure?

If you're a bank then theconsequenceof failure is pretty bad. If you're a service like twitter then it isn't too bad. For a long time twitter was super unreliable, but they got through it and became highly successful.

<div class="wp-caption aligncenter" id="attachment_2459" style="width: 310px">[![Remember this guy?](http://stimms.files.wordpress.com/2013/03/failwhale.jpg?w=300)](http://stimms.files.wordpress.com/2013/03/failwhale.jpg)Remember this guy?

</div>Test the portions of your application which would have the greatest impact if they were broken. Are you reliant on processing invoices? Then invoice processing should be the focus of your testing. Are you a service which authenticates users? Then that should be the focus of your testing.

Another approach is to test the parts of your application which change the most frequently. The idea being that the parts which change the most frequently are unlikely to have the same level of exposure to user testing as the rest of the site. Changing stable code is necessary but likely to introduce problems.

I don't know what the solution is to how much testing needs to be done. As with all difficult problems I feel like a single solution is over simplifying the problem.

No testing? Test everything? Somewhere in the middle.



