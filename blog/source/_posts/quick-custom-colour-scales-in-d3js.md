---
layout: post
title: Quick Custom Colour Scales in d3js
authorId: simon_timms
date: 2013-07-08
---

The built in color scales in d3 are wonderful tools for those of us who aren't so good with coming up with colour schemes. I would love to have the skills to be a designer but by now it should be pretty clear that's never going to happen. However it occurred to me the other day that the built in scales in d3 are designed for high contrast rather than for colour scheme consistency.

<div class="wp-caption aligncenter" id="attachment_2912" style="width: 516px">[![Woah, consistent](http://stimms.files.wordpress.com/2013/07/6542_04_04.png)](http://stimms.files.wordpress.com/2013/07/6542_04_04.png)Woah, consistent

</div>In order to make prettier graphs and visualizations I thought I would build a colour scheme scale. I started with a very simple two colour scale

<script src='https://gist.github.com/stimms/5951224.js'></script>

This can be used as a drop in replacement for the d3 colour scale. You can specify a domain and a range then call it as you would a normal d3 scale

<script src='https://gist.github.com/stimms/5951337.js'></script>

I used it to alternate colors in my graph to look like this

<div class="wp-caption aligncenter" id="attachment_2913" style="width: 508px">[![Prettier](http://stimms.files.wordpress.com/2013/07/sensible-colors.jpg)](http://stimms.files.wordpress.com/2013/07/sensible-colors.jpg)Prettier

</div>It would be trivial to change the scale to account for more colours

<script src='https://gist.github.com/stimms/5951372.js'></script>#file-gistfile1-txt-L7

Now you can make graphs which look like

<div class="wp-caption aligncenter" id="attachment_2914" style="width: 504px">[![More columns for more column fun!](http://stimms.files.wordpress.com/2013/07/sensible-colors2.jpg)](http://stimms.files.wordpress.com/2013/07/sensible-colors2.jpg)More columns for more column fun!

</div>Of course color scales can be used for all sorts of fun applications. Consider

- highlight bars which are above or below a certain threshold
- display another dimension of data
- be switchable for the color blind

Anyway this was just a fun exercise to see what I could come with in a couple of minutes.



