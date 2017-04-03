---
layout: post
title: Parsing HTML Easily in C#
authorId: simon_timms
date: 2013-06-05
---

<div style="background:#f3f3f3;">If you're interested in parsing HTML in C# using CSS3 selectors then check out my [followup post](http://blog.simontimms.com/2014/02/24/parsing-html-in-c-using-css-selectors/ "Parsing HTML in C# Using CSSSelectors") on the topic.</div>In an ideal world every website would have a friendly JSON interface which would allow you to, at the very least, read data. We sure don't live in that world. For a couple of the windows 8 and windows phone apps with which I've been playing I've needed to go out to a website and parse some content. I started off in python using a library called BeautifulSoup. I got that all working fine but I ran into a problem inserting the records I had retrieved into an SQL Azure database. As it turns out there are some silly inconsistencies with using FreeTDS on OSX which would have required me to recompile to get it to work against Azure SQL.

Nope.

There was a time when I was all over building my own kernel and tweeking things. I'm past that now and I just want stuff to work. God, just work (I have a whole rant about printers and printing not working but it descends into language which would melt your face so quickly that I dare not post it). So that went out the Window and I switched to C#. Actually, first I switched to using a JavaScript scheduled task in Azure mobile services. However there were limited node modules available and I didn't feel like writing or including a whole HTTP library.

Right. I chose to use the [HTML agility pack](http://htmlagilitypack.codeplex.com/) which is a pretty good parser for HTML. Frequently sites don't have well formed HTML which makes parsing them with a full featured XML parser impossible. You don't want to get involved in XML anyway - it is a gateway drug to proprietary file formats.

The agility pack has a built in HTTP client but I wasn't too impressed with it as it just returned the document and not the status code. Without knowing if the page was a 404 my particular parsing job was difficult. Instead I made use of the HTTP client in System.Net.Http to pull the page. In this case I'm pulling down a page from the web comic XKCD.

<script src='https://gist.github.com/stimms/5719169.js'></script>

All this stuff is asynchronous in .net 4.5 which is great. For all too long we've laboured in code bases which assume that network operations run at local speeds. I'm looking at you windows file system.

Parsing the returned document is also very simple

<script src='https://gist.github.com/stimms/5719194.js'></script>

Now we have the document loaded into the agility pack we can perform simple XPath queries against it.

<script src='https://gist.github.com/stimms/5719209.js'></script>

The first query there selects the first image in the first div with an id attribute with value comic. Using a double // means that I'm not concerned about how deep in the tree the node appears. The second query just grabs the first div with an id of ctitle. You can also use a SelectNodes function to select multiple matching nodes.

I'm not crazy about using xpath for this sort of thing. It would be great to have a CSS selector based library in C#. As it turns out there is a project called [Fizzler](https://code.google.com/p/fizzler/). I guess it is still in beta, but the few things I read about it suggest it works quite well. I'll have to play with it for another post. It certainly would be nice to only have to know one HTML query language.



