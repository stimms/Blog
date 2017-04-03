---
layout: post
title: Force Directed Graph 2
authorId: simon_timms
date: 2013-03-01
---

Yesterday I showed creating a force directed graph with a dataset taken from wikipedia. As it stood it was pretty nifty but it needed some interaction. We're going to add some very basic interaction. To do this we'll use the data- attribute we added when building the graph. These attributes were added as part of HTML5 to allow attaching data to DOM elements. Any attributes which are prefixed with data- are ignored by the browser unless you specifically query for them.

I started by adding a set of buttons to the page, one for each show.

<script src='https://gist.github.com/stimms/5069090.js'></script>

Then I added a simple jQuery function to hook up these buttons and filter the graph

<script src='https://gist.github.com/stimms/5069097.js'></script>

First I reset the graph to is original color, then I select a collection of elements using the data-productions element. This is a stunningly inefficient way to select elements but we have a pretty small set of data with which we're working so I'm not overly concerned about the efficiency. It could be improved by using a css class instead of a data-attribute as these selectors have been optimized.

The final product is up at[http://bl.ocks.org/stimms/5069532](http://bl.ocks.org/stimms/5069532)

There are a bunch of other fun customizations we could do to this visualization. Some of my ideas are:

- <span style="line-height:13px;">Improve the display of name labels during hover</span>
- Highlight linksbetweenpeople when you hover over a node
- Show the number of collaborations when somebody hovers over a link

That's the best part of visualization: there is always some crazy new idea to try out. The trick is to know when to stop and what you can take away without ruining the message.



