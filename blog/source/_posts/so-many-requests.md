---
layout: post
title: So Many Requests
authorId: simon_timms
date: 2013-02-01
---

I make use of wordpress.com for my blogging. I chose it because it was easy, andrelativelycheap. I could have created my ownWordpresssite on Azure or Amazon but it is actually far less economical ($99 a year vs. $570 a years for even the smallest Amazon instance). One really nice feature is that there is an awesome looking page which lists various statistics about your blog. I tell people I don't care about how many people read the blog but, damn it, if watching that page isn't addictive.

The page looks like this:

[![stats](http://stimms.files.wordpress.com/2013/01/stats.png)](http://stimms.files.wordpress.com/2013/01/stats.png)

I've been on a bit of a vacation for the last week and have had poor internet access and the page seems to take a long time to load. I cued up the network monitor in chrome to see why the page was taking so long to load. The results were shocking to me: the page required 122 server requests. That's is anawfullot. When I build sites I'm pretty worried at 10 server trips. 10 server trips is a smell. 122 is stinky

[![Lawrence Limburger](http://stimms.files.wordpress.com/2013/01/lawrence_limburger.png)](http://stimms.files.wordpress.com/2013/01/lawrence_limburger.png)

Digging into these requests we find that 27 of the requests are JavaScript and another 10 of them are CSS. The majority of the reset are images, we'll come to those later. Why is it a big deal that there are so many requests? Because there significant overhead to both performing DNS lookups and to opening up so many HTTP requests. You can only open a limited number of requests at a time so some of these many server trips happen in serial. It all adds up to slow page loads and a poor user experience.

The solution for the JavaScript and CSS is easy to implement: bundle the requests. A lot of work has gone on in the .net space to enable [bundling of files](http://www.asp.net/mvc/tutorials/mvc-4/bundling-and-minification). It has been a while since I've done work on a Linux stack but it looks like WordPress runs on Nginx as a webserver. As it turns out there is a [concatenationmodule](https://github.com/taobao/nginx-http-concat) for Nginx. Why the heck isn't wordpress, which is a major website serving files which are so unoptimized? I don't know, perhaps a lack of craftmanship.

This is an easy win: a couple of hours work implementing a concatenation module saves eery one of your users time. Come one, WordPress. Let's get with the program.

Now for images: combining images is actually pretty easy. You can make use of [CSS sprites](http://css-tricks.com/css-sprites/). Basically all the images on the page are combined into one image and then the client cuts them up into their appropriate sizes. This page is a bit problematic because a lot of the images are gravatars. Gravatar is a service which will transform e-mail addresses into avatars. It is used all over the place now days. The issue is that it only handles one image at a time. So if your page has 40 comments on it, and each comment has a gravatar you're instantly at 40 server trips. If you're serious about having gravatars on your site and still having a good user experience what I would suggest is that you build a gravatar proxy. This service would make the gravatar requests on the server side, bundle them as a single image and return it to the client which could split the image. With a bit of a caching policy on the server side you could really reduce the number of trips out to gravatar too.

With these improvements the page would load faster and I would be much happier. We're talking about no more than a handful of hours work. Come on, wordpress, get with it.

Oh, and in answer to my wife's question about who the guy in the purple suit is: he is Lawrence Limburger evil mastermind from the cartoon Biker Mice from Mars. There was a running gag that he smelled. You're always going to learn something at this blog, although it might not be what you expect.



