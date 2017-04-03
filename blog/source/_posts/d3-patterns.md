---
layout: post
title: d3 Patterns
authorId: simon_timms
date: 2014-07-17
---

I'm a big fan of the d3 data visualization library to the point where I wrote a [book ](http://www.amazon.com/Social-Data-Visualization-HTML5-JavaScript/dp/1782166548/ref=sr_1_4?ie=UTF8&qid=1405608079&sr=8-4&keywords=simon+timms)about it. Today I came across and interesting problem with a visualization I'd created. I had a bunch of rows which I'd colored using a 10 color scale.[![rows](http://stimms.files.wordpress.com/2014/07/rows.png)](https://stimms.files.wordpress.com/2014/07/rows.png)



The users wanted to be able to click on a row and have it highlight. Typically I would have done this by changing the color of the row but I had kind of already used up my color space just building the rows. I needed some other way to highlight a row. I tried setting the border on the row but that looked ugly and became a tangled mess when adjacent rows were highlighted.

[![rows2](http://stimms.files.wordpress.com/2014/07/rows2.png)](https://stimms.files.wordpress.com/2014/07/rows2.png)

What I really wanted was to put some sort of a pattern on the row. As it turns out this is quite easy to do. SVG already provides a mechanism for applying patterns as fills. The one issue is that you can't apply a pattern as an overlay to an existing fill you have to replace it completely.

First I created the pattern in d3

<script src='https://gist.github.com/stimms/89ba123b1a11a26d1ce5.js'></script>

Here I create a new pattern element and put a rectangle in it. I rotate the whole pattern on a 45 degree angle to get a more interesting pattern. You may notice that the code references the variable d. I'm actually creating and applyingthis pattern inside of a click handler for the row. This allows me to create a new pattern for each row and color it correctly. The full code looks like

<script src='https://gist.github.com/stimms/0fa82ffafa9d799887a2.js'></script>

The finished product looks like

[![rows3](http://stimms.files.wordpress.com/2014/07/rows3.png)](https://stimms.files.wordpress.com/2014/07/rows3.png)

You can change the pattern to come up with more interesting effects

[![rows4](http://stimms.files.wordpress.com/2014/07/rows4.png)](https://stimms.files.wordpress.com/2014/07/rows4.png)

[![rows5](http://stimms.files.wordpress.com/2014/07/rows5.png)](https://stimms.files.wordpress.com/2014/07/rows5.png)



