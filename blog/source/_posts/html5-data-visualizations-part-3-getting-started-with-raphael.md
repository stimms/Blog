---
layout: post
title: HTML5 Data Visualizations "“ Part 3 "“ Getting Started with RaphaÃ«l
authorId: simon_timms
date: 2013-01-11
---

<span style="background-color:lightyellow;border-color:#E6DB55;border:solid 1px;display:block;">**Note**: I will be presenting a talk on data visualization in HTML5 on February the 12th at the Calgary .net user group. Keep an eye on[http://www.dotnetcalgary.com/](http://www.dotnetcalgary.com/) for details. This blog is one in a series which will make up the talk I will be giving.</span>

In the last part of this series we looked at building a basic graph using static SVG. This approach is a little limited in that it is painful to build up this markup on the server side. I mentioned that SVG elements are part of the DOM and this is true but it isn't as easy to manipulate the items as you might think.

Here is some code which seems like it would add a new circle to the SVG

<script src='https://gist.github.com/4516134.js'></script>

But if you run that on the page that we created in part 2 the expected circle doesn't appear. What's going on? Well even though the circle is appended to the SVG when we examine the DOM in the developer tools in Chrome. Well it is a bit weird but despite the fact that the code exists in the same document it is actually in a different name space so the circle we add is not a real SVG circle. There are some hacks to get around it listed in this [great Stackoverflow question](http://stackoverflow.com/questions/3642035/jquerys-append-not-working-with-svg-element). Instead of these hacks you can use a graphing library like Raphael. Technically the e in Raphael has an umlaut on it but I say we didn't take the birch cane out and give the Germans a right proper thrashing so they can go about inventing characters.

Raphael.

Ralph.

Jolly. good. show.

Raphael is a library for manipulating SVGs and easily creating drawings. It is great for the sorts of data visualizations we're interested in. Let's go about recreating out graph from part II using Raphael.

<script src='https://gist.github.com/4516275.js'></script>

This will create a single bar just like we did previously. You can see that the methods for Raphael look pretty similar to the SVG native commands. Now the advantage of using javascript is that we can write functions to make our lives easier.

<script src='https://gist.github.com/4516328.js'></script>

Here we've made use of some data which could be json sourced from the server or from some other API. We've also done some simple math to figure out appropriate column heights. This makes everything far moreexpandable. If we want more columns then all that is needed is to add more to the data array. In this example code we haven't accounted for recalculating the width of the columns so this graph will become wider as more columns are added.[![Screen Shot 2013-01-11 at 11.03.36 PM](http://stimms.files.wordpress.com/2013/01/screen-shot-2013-01-11-at-11-03-36-pm.png?w=300)](http://stimms.files.wordpress.com/2013/01/screen-shot-2013-01-11-at-11-03-36-pm.png)

You might notice that the last column there is clipped. That's because we have more columns than can be fit into the size of the SVG we created.

We can easily add labels with just one more line, in this case line 26.

<script src='https://gist.github.com/4516393.js'></script>

[![Screen Shot 2013-01-11 at 11.21.21 PM](http://stimms.files.wordpress.com/2013/01/screen-shot-2013-01-11-at-11-21-21-pm.png?w=300)](http://stimms.files.wordpress.com/2013/01/screen-shot-2013-01-11-at-11-21-21-pm.png)

We can extract the javascript into a component and use it all over our site. We'll look at doing that in another part of the series. We'll probably see what we can do about cleaning up the javascript too, I don't like the look of those functions.



