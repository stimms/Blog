---
layout: post
title: HTML5 Data Visualizations - Part 1 - SVG vs. Canvas
authorId: simon_timms
date: 2013-01-07
---

<span style="background-color:lightyellow;border-color:#E6DB55;border:solid 1px;display:block;">**Note**: I will be presenting a talk on data visualization in HTML5 on February the 14th at the Calgary .net user group. Keep an eye on[http://www.dotnetcalgary.com/](http://www.dotnetcalgary.com/) for details. This blog is one in a series which will make up the talk I will be giving. </span>

Sometimes I look back at theexcellentinternet archive and laugh about what bit websites looked like a few years ago. Take a look at [CNN from the year 2000](http://web.archive.org/web/20000815052826/http://www.cnn.com/)or [Blizzard from the year 1998](http://web.archive.org/web/19981207025934/http://209.67.136.168/)in comparison with these same sites today the quality of the design is dreadful. There has been a real revolution in the quality of the sites on the Internet. I'm delighted that this is the case but it does raise the bar for those of us who build websites. Everybody expects our sites to be more attractive and for data to be presented in a more useful fashion than it has been in the past.Fortunatelya number of the key improvements in HTML5 have been around presentation of data.

If you talk to anybody I've worked with they'll tell you that I love graphs. I love the little guys. I love bar charts, I love pie charts, I love spark lines, I don't love gauges, but I love line charts. I love them so much I want them on my [receipts](http://blog.simontimms.com/2013/01/03/startup-idea-geek-proxies/ "Startup Idea: GeekProxies"). HTML5 offers a couple of mechanisms for creating drawings(which is what graphs are): Canvas and SVG.

Each of these has their advantages which is why theargumentsI've seen people having about which one is better overall to be a bit silly. Sometimes a spoon is better and sometimes a fork is better. a spork is almost always the worst solution, but that might just be because I'm sporkist.

Canvas is a raster format which means that you canmanipulateimages in it very easily. If you were looking to build a graphics editor or perform analysis of an existing image then canvas would be for you. Also if you were looking to build a game this is most commonly done using canvas because of the difficult nature of building game graphics in a vector format. As a graphics professor of mine use to say â€œRaster is faster, but vector is betterâ€. There is pretty good support for canvas on major browsers and even in [older versions](http://caniuse.com/#feat=canvas). If you're trapped in a company where the IT department have totally failed to keep up with the times and are still running Windows XP there is some hope, you can use [excanvas](http://excanvas.sourceforge.net/)to add support to older versions of IE. It is a lot slower and doesn't render things perfectly. You should probably sigh loudly and shake your head in woe that your company hasn't grasped how important IT is.

Now for SVG. These are scalable vector graphics which means that the graphic is made up from a series of lines and curves. It is surprising how impressive the things are you can build with what amounts to a pile of shapes. Being made of a bunch of lines means that you can scale the images up and down as you see fit. So if you want to zoom into a section of the graphic it can be done without losing any of the quality as you would see in a raster image. I like to think of them as the format in which all video recorders record in most crime dramas. Browser support for SVG is about the same as [canvas](http://caniuse.com/#feat=svg). As most charts and graphs are made up of lines and the such this seems like an ideal solution.

SVG data is stored as part of the DOM which means that it can be manipulated with standard javascript and that you can easily assign action listeners to objects within the canvas.Add to this is the fact that SVGs support declarative animation and you've got yourself a graphics solution for data visualization in HTML5.

You can read a bunch more about SVG vs. Canvas in a number of places like [here](http://dev.opera.com/articles/view/svg-or-canvas-choosing-between-the-two/), [here](http://msdn.microsoft.com/en-us/library/ie/gg193983(v=vs.85).aspx)and [here](http://blog.safaribooksonline.com/2012/04/17/using-html5-canvas-vs-svg/).

In the next part we'll look at how to use SVG.



