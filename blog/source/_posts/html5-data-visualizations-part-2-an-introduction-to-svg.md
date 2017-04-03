---
layout: post
title: HTML5 Data Visualizations â€“ Part 2 â€“ An Introduction to SVG
authorId: simon_timms
date: 2013-01-09
---

<span style="background-color:lightyellow;border-color:#E6DB55;border:solid 1px;display:block;">**Note**: I will be presenting a talk on data visualization in HTML5 on February the 14th at the Calgary .net user group. Keep an eye on[http://www.dotnetcalgary.com/](http://www.dotnetcalgary.com/) for details. This blog is one in a series which will make up the talk I will be giving.</span>

In part 1 we took a look at canvas vs SVG for building graphs and data visualizations. We decided that SVG would probably be better for our purposes. We can build up SVGs by hand or by using a library like jQuery after all they are simply part of the DOM. If you remember SVGs are made up of a series of simple shapes. So let's build a few little visualizations with SVG. How about a bar chart to start with? A bar chart is made up of a series of rectangles of various different heights.

First thing we need to do is include the SVG in the page. This can be done a number of ways. You can link to an .svg file, you can put it in an iframe or you can embed it directly on the page. I like embedding on the page as it saves a server trip. Now unfortunately the blogging platform I'm using doesn't support SVG, so I'll put in raster images instead of the SVG. There are some [potential security issues](http://cve.mitre.org/cgi-bin/cvekey.cgi?keyword=svg) with SVG so that is likely the reason the platform doesn't support them. These issues will be ironed out just as security issues with HTML were ironed out years ago.

To start with we need to include an svg tag in the document and give it a few instructions

<script src='https://gist.github.com/4499472.js'></script>

This renders a single bar of our bar chart. The code is pretty easy to understand. The recttag creates s rectangle with a width of 50 and a height of of 150 offset 20 pixels from the top left corner of the SVG. It looks like this:  
  
[![Screen Shot 2013-01-09 at 9.22.22 PM](http://stimms.files.wordpress.com/2013/01/screen-shot-2013-01-09-at-9-22-22-pm.png?w=163)](http://stimms.files.wordpress.com/2013/01/screen-shot-2013-01-09-at-9-22-22-pm.png)

Weeee! Okay so what next? Let's build up the SVG necessary for a full graph. We're going to need a few bars as well as a legend and an axis. Let's start with a few bars

<script src='https://gist.github.com/4499685.js'></script>

Here we've just added a few more rectangles and reduced the height of each. Because the origin (0,0) is in the top left we also have the change the y value for each bar. This generates

[![Screen Shot 2013-01-09 at 10.26.51 PM](http://stimms.files.wordpress.com/2013/01/screen-shot-2013-01-09-at-10-26-51-pm.png?w=300)](http://stimms.files.wordpress.com/2013/01/screen-shot-2013-01-09-at-10-26-51-pm.png)

Now let's add the axes. To do this we add a couple of lines to our SVG

<script src='https://gist.github.com/4499703.js'></script>

[![Screen Shot 2013-01-09 at 10.34.06 PM](http://stimms.files.wordpress.com/2013/01/screen-shot-2013-01-09-at-10-34-06-pm.png?w=300)](http://stimms.files.wordpress.com/2013/01/screen-shot-2013-01-09-at-10-34-06-pm.png)

We're almost done here. Let's add some labels and a title.

<script src='https://gist.github.com/4499751.js'></script>

Here you can see we're adding a series of X and Y axis labels as well as the title. Placing text in SVG so it looks nice in relation to the other elements is difficult. If we were programatically adding elements to the SVG we could calculate the length of the text and position it properly.

[![Screen Shot 2013-01-09 at 10.51.48 PM](http://stimms.files.wordpress.com/2013/01/screen-shot-2013-01-09-at-10-51-48-pm.png?w=300)](http://stimms.files.wordpress.com/2013/01/screen-shot-2013-01-09-at-10-51-48-pm.png)

Now we have a full graph with a title, labels and the introduction of a kick-ass new month. I'm also comforted to know that there is always at least two monsters under the bed. It is when there is only one they get board and devour your young. True story.

All together the code looks like

<script src='https://gist.github.com/4499795.js'></script>

In the next post we'll look at using some libraries to make our job easier.



