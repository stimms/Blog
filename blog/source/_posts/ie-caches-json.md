---
layout: post
title: IE Caches JSON
authorId: simon_timms
date: 2009-09-16
---

I ran into an interesting problem today on everybody's favorite browser Internet Explorer. At issue was a page I had which was partially populated using jQuery's [getJSON](http://docs.jquery.com/Ajax/jQuery.getJSON) function. As it turns out even though I had the caching turned to no-cache on the server IE was perfectly happy to cache the document because it was fetched using GET. Apparently this is OK to do. Obviously this ruins my site's functionality so I instructed jQuery to override it by setting

  
 $.ajaxSetup({ cache: true });

This function works by adding a nonsense value to the end of the request. Looking at the actual jQuery source we can see that the current time is appened to the request which makes the browser believe it is a new URL.

  
 if ( s.cache === false && type === "GET" ) {  
 var ts = now();  
  
 // try replacing _= if it is there  
 var ret = s.url.replace(rts, "$1_=" + ts + "$2");  
  
 // if nothing was replaced, add timestamp to the end  
 s.url = ret + ((ret === s.url) ? (rquery.test(s.url) ? "&" : "?") + "_=" + ts : "");  
 }

Problem solved



