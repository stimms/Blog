---
layout: post
title: Progress Bars in PowerPoint
authorId: simon_timms
date: 2013-01-15
---

From time to time I find that I have to create PowerPoint presentations. I don't like it and I'd much rather use a tool like [Impress.js](http://bartaz.github.com/impress.js/)or [deck.js](http://imakewebthings.com/deck.js)or better yet not use slides at all.Unfortunatelypeople have grown to expect slides and while javascript options are cool they aren't maintainable by the non-programmers in my group. One of the pieces of advice my wife gave me about presentations is to put the slide number and total number of slides on the bottom of each slide.

> â€œIf you're going to make people suffer through your presentations you can at least give them the hope of knowing how much longer it will lastâ€

-My Wife

She isn't wrong. I wanted to make my presentation a little bit more interesting than the average so I googled for a progress bar for powerpoint. I found [a site](http://en.kioskea.net/faq/937-insert-a-progress-bar-to-powerpoint-presentation) which listed how to do it. I modified the code a little so it picks up the schema from the current slide master and has a page number on it.

<script src='https://gist.github.com/4544975.js'></script>

You can just copy this into a new module into your PowerPoint's code behind(hit Alt-F11) and then run it with the little green run arrow. You'll need to run it again every time you change the slide count.

It comes out with a progress bar at the bottom of each slide which looks like

[![progress](http://stimms.files.wordpress.com/2013/01/progress.png)](http://stimms.files.wordpress.com/2013/01/progress.png)

Now you'll never have to wonder if I am almost done my ranting.





