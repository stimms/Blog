---
layout: post
title: Parsing HTML in C# Using CSS Selectors
authorId: simon_timms
date: 2014-02-24
---

A while back I [blogged](http://blog.simontimms.com/2013/06/06/parsing-html-easily-in-c/ "Parsing HTML Easily inC#") about parsing HTML in C# using the [HTML Agility Pack](http://htmlagilitypack.codeplex.com/). At the end of that post I mentioned that the [fizzler](https://code.google.com/p/fizzler/) library could be a better way of selecting elements in HTML. See the Agility Pack uses XPath queries to find elements which match selectors. This is contrary to the CSS3 style selectors which we're use to using in jQuery's Sizzle engine.

For instance in XPath to find the comic image on an XKCD page we used

 //div[@id='comic']//img

using a CSS3 selector we simply need to do

 #comic>img

This is obviously far more terse and yet easy to understand. I'm not sure who designed these selectors but they are jolly good. Unfortunately not all of the CSS3 selectors are supported, however I didn't find a gaping hole when I tried it. Fizzler is actually built on the HTML Agility Pack so if you're really stuck with a CSS3 query which doesn't work then you can drop back to using simple XPath.

So if we jump back into the same project we had before then we can replace the XPath queries

<script src='https://gist.github.com/stimms/5719209.js'></script>

with

<script src='https://gist.github.com/stimms/9128851.js'></script>

For queries which are as simple as the ones here either XPath or CSS3 aren't that different. However you can build some pretty complicated queries which are much more easily represented in CSS3 selectors than XPath. I would certainly recommend Fizzler now because of the general familiarity with CSS3 selectors that jQuery has brought to the development community.



