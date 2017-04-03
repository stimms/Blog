---
layout: post
title: Caching and Why You Shouldn't Listen to Blogs
authorId: simon_timms
date: 2009-08-28
---

A while ago I wrote and article entitled â€œ[HTML Helper for Including and Compressing Javascript](http://stimms.blogspot.com/2009/06/html-helper-for-including-and.html)â€œ, no don't click on that link because it is all wrong. The gist of the article was that in order to save clients from downloading a bunch javascript files each time they visited the site opening up a bunch of costly connections a handler would include all those files in the HTML page and, as an added bonus, compress them. I forgot one key thing and [leddt](http://leddt.myopenid.com/) was good enough to point it out. Because each page you load on the site has the javascript inlined there is no way to cache it.

So what do we do? I think the best solution is to stop compressing the javascript on request. Instead combine and compress the javascript as part of the build process and then serve it up as a separate request. The disadvantage here is that if you use a library like jquery-ui on only one page you end up downloading it for any page the user visits. However the price you pay in getting that is a one time cost while using the terrible solution I suggested before you pay for it again and again and again. In that way it is much like not taking out the trash, I have to hear about it every day plus it smells. You wouldn't believe how hard it is to get the smell out of the fur of the 40 dogs in the puppy mill I run in my garage.

How things are cached on a browser has always been a mystery to me and there isn't a whole lot on the internet about the technicalities of what browsers do and do not cache. Basically it seems to come down to the cache-control header which govern how devices retain content. The ever so verbose W3C HTTP 1.1 spec defines the grammar for cache-control as

  
Cache-Control = "Cache-Control" ":" 1#cache-directive  
 cache-directive = cache-request-directive  
 | cache-response-directive  
 cache-request-directive =  
 "no-cache" ; Section 14.9.1  
 | "no-store" ; Section 14.9.2  
 | "max-age" "=" delta-seconds ; Section 14.9.3, 14.9.4  
 | "max-stale" [ "=" delta-seconds ] ; Section 14.9.3  
 | "min-fresh" "=" delta-seconds ; Section 14.9.3  
 | "no-transform" ; Section 14.9.5  
 | "only-if-cached" ; Section 14.9.4  
 | cache-extension ; Section 14.9.6  
 cache-response-directive =  
 "public" ; Section 14.9.1  
 | "private" [ "=" 1#field-name ] ; Section 14.9.1  
 | "no-cache" [ "=" 1#field-name ]; Section 14.9.1  
 | "no-store" ; Section 14.9.2  
 | "no-transform" ; Section 14.9.5  
 | "must-revalidate" ; Section 14.9.4  
 | "proxy-revalidate" ; Section 14.9.4  
 | "max-age" "=" delta-seconds ; Section 14.9.3  
 | "s-maxage" "=" delta-seconds ; Section 14.9.3  
 | cache-extension ; Section 14.9.6  
 cache-extension = token [ "=" ( token | quoted-string ) ]

Pretty simple. The fields you have to watch out for are max-age and public/private. The age is how long the cache is permitted to retain the document before it must rerequest it and public indicates the page is public and should be cached for all users. How the actual browsers will implement these is a function of the users so all you can do is make sure your site obeys the standard.

But don't trust me, I'm just a blogger and I already lied to you once. In my next post I'll talk a bit about caching in ASP.net and how to save database trips.



