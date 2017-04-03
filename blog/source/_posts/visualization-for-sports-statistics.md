---
layout: post
title: Visualization for Sports Statistics
authorId: simon_timms
date: 2013-06-06
---

Having moved to Canada late in life I never really got into the hockey spirit. It might have been that hockey looked hard or it might have been a strategic campaign of hockey disinformation perpetrated by my parents in order to avoid driving to Tiny Town in the Middle of Nowhere, Alberta at 6am on a Saturday. If the latter then it is a brilliant plan I intend to duplicate with my children. Ideally they will play a sport which enables me to travel to warm location and sit on a beach while they do whatever they do. Competitive tiddlywinks, perhaps.

Despite this lack of interest in hockey I do find myself on the NHL website several times a month. Partially my visits are due to a well developed sense of schadenfreude around the pathetic Calgary Flames. Partially they are due to a love of statistics. I like watching the numbers of points move and the teams go up and down and calculating just how hopeless the Flames are.

I don't think I'm the only one who likes sports statistics but the presentation of these statistics is really dry. A table of numbers is, generally, not very accessible to people. I thought I might try building a few simple visualizations to try to improve upon that. If you happen to be a representative of some professional sporting organization which pays their players millions of dollars and you want to steal these visualizations then they are ultra-mega-super-purple-ninja copyrighted(we can work out a massive license agreement). If you're anybody else then they're open source.

Today I'm going to look at a best of series. These are very common and are typically in the form of the first team to 3 wins wins the series. You know, unless you're the NHL and stretch the season out into July by making your series best of 7. On nhl.com tonight there is a graphic showing how the teams are doing. We're down to 4 teams so these are the semi-finals.

<div class="wp-caption aligncenter" id="attachment_2811" style="width: 719px">[![Are they're even any living kings who play hockey?](http://stimms.files.wordpress.com/2013/06/nhl.jpg)](http://stimms.files.wordpress.com/2013/06/nhl.jpg)Are they're even any living kings who play hockey?

</div>The key information for which people are looking on an image like this is:

1. which teams are playing

2. who is winning the series

3. the next game time

The graphic they use is pretty good at giving the teams playing. It is clear which teams are playing against each other and, if you're fan enough to go to nhl.com, the logos are obvious. The next game in the series is also obvious. However the number of games won by each team in the series isn't. My eye is drawn to the number under the team abbreviation. This is actually the standing of the team in the regular season. For the series on the left it is relatively apparent this isn't the information for which I'm looking: there can be no one team with 5 wins. The series on right is less obvious. Boston could well be leading 4-1.

Instead of putting in "BOS LEADS 3 "“ 0"³ we could put in an easy visualization.

<div class="wp-caption aligncenter" id="attachment_2812" style="width: 715px">[![Eeek, I am not good at paint](http://stimms.files.wordpress.com/2013/06/nhl1.jpg)](http://stimms.files.wordpress.com/2013/06/nhl1.jpg)Eeek, I am not good at paint

</div>There are some improvements possible here too:

- use the team colours to fill the boxes
- use a gradient to show how each game gets closer to hitting the magic 4
- make it more obvious that they only have to get to half way
- change the shade of the fill depending on the score differential

You can check out my d3.js version of this over at[http://bl.ocks.org/stimms/5727304](http://bl.ocks.org/stimms/5727304)



