---
layout: post
title: pointer-events -  Wha?
authorId: simon_timms
date: 2013-04-04
---

Every once in a while I come across something in a technology I've been using for ages which blows my mind. Today's is thanks to Interactive Data Visualizations for the Web about which I spoke [yesterday](http://blog.simontimms.com/2013/04/04/review-interactive-data-visualization-for-the-web/ "Review: Interactive Data Visualization for theWeb").In the discussion about how to set up tool tips there was mention of a CSS property called pointer-events. I had never seen this property before so I ran out to look it up. As presented in the book the property can be used to prevent mouse events from firing on a div. It is useful for avoiding mouse out events should a tool tip show up directly under the user's mouse.

The truth is that it is actually a far more complex property. See an SVG element is constructed from two parts: the fill is the stuff inside theelement In the picture here is is the purple portion. There is also a stroke which is theouteredge of the shape; shown here in black.

[![Screen Shot 2013-01-09 at 9.22.22 PM](http://stimms.files.wordpress.com/2013/01/screen-shot-2013-01-09-at-9-22-22-pm.png)](http://stimms.files.wordpress.com/2013/01/screen-shot-2013-01-09-at-9-22-22-pm.png)

By setting pointer-events to fill then mouse events will only fire when the cursor is over the filled portion of an element. Conversly setting it to stroke will only fire events when the mouse pointer is over the outer border. There are also settings to only fire events for visible elements (visibleStroke, visibleFill) and to allow the mouse events to pass through to the element underneath (none). The[Mozilla documents](https://developer.mozilla.org/en/docs/CSS/pointer-events)go into its behaviour in some depth.

The ability to hide elements from the mouse pointer is powerful and can be used to improve the user's interaction with mouse events. I had originally recommended using an invisible layer for this but pointer-events is much cleaner.



