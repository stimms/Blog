---
layout: post
title: Improving the Best of Visualization
authorId: simon_timms
date: 2013-06-07
---

If you read the comments on [yesterday's post](http://blog.simontimms.com/2013/06/07/visualization-for-sports-statistics/ "Visualization for SportsStatistics") there was a bit of a discussion about some alternatives to the number of boxes in the best of visualization I created. When I built that visualization I based it on the number of games + 1. This would prevent a situation where a full series was required and the final score was ambiguous

<div class="wp-caption aligncenter" id="attachment_2818" style="width: 544px">[![Sorry, who won that last game?](http://stimms.files.wordpress.com/2013/06/bestof1.jpg)](http://stimms.files.wordpress.com/2013/06/bestof1.jpg)Sorry, who won that last game?

</div>By using colours it is possible to make this look a bit better. This also offers us an opportunity to generalize some of the code written yesterday.

The most obvious improvement is around the function shouldBeFilled

<script src='https://gist.github.com/stimms/5734114.js'></script>

First thing is that the function name is lying. It isn't telling if the box should be filled or not. It is setting the fill color. So let's rename it. Next up the use of hard coded values for the colours bugs me. If we're going to be using a different colour for each team then we have to change this.

<script src='https://gist.github.com/stimms/5734119.js'></script>

Here I've gone ahead and assumed that I have an options object in my class. I don't so one will need to be added. I'd like to maintain backwards compatibility so I'll set up some default values for options.

<script src='https://gist.github.com/stimms/5734141.js'></script>

Nobody calling my needs to change their signature but they can still override the options if they wish. $.extend is a jQuery helper which merges the properties on two objects. One could even say that it composes a new object.

<div class="wp-caption aligncenter" id="attachment_2819" style="width: 540px">[![It is clear which team won now](http://stimms.files.wordpress.com/2013/06/bestof2.jpg)](http://stimms.files.wordpress.com/2013/06/bestof2.jpg)It is clear which team won now

</div>To set the options a caller can just do

<script src='https://gist.github.com/stimms/5734150.js'></script>

<div class="wp-caption aligncenter" id="attachment_2820" style="width: 540px">[![Guess which one uses pink](http://stimms.files.wordpress.com/2013/06/bestof3.jpg)](http://stimms.files.wordpress.com/2013/06/bestof3.jpg)Guess which one uses pink

</div>There was also talk of adding some emphasis to the center block. I do have some ideas for that which we'll explore in a future post. You can see the results of today's post at[http://bl.ocks.org/stimms/5734160](http://bl.ocks.org/stimms/5734160)



