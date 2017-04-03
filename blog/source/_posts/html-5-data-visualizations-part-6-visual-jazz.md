---
layout: post
title: HTML 5 Data Visualizations â€“ Part 6 â€“ Visual Jazz
authorId: simon_timms
date: 2013-01-29
---

<span style="background-color:lightyellow;border-color:#E6DB55;border:solid 1px;display:block;">**Note**: I will be presenting a talk on data visualization in HTML5 on February the 14th at the Calgary .net user group. Keep an eye on[http://www.dotnetcalgary.com/](http://www.dotnetcalgary.com/) for details. This blog is one in a series which will make up the talk I will be giving.</span> I'm planning for this to be the finalinstillmentof this series. However, I've enjoyed playing with d3.js so much that I will very likely make visualization using it an ongoing theme on this blog. I've never considered myself much of an artist, as my poor school teachers can attest, but I do like this visualization design. In the last part of the series we figured out how to make a simple bar chart using d3.js. But this isn't going to impress your boss because your boss read an article last week about HTML5 and how it is better than excel(I swear to you there are articles like this in â€œBoss Magazineâ€ and â€œPointy Hair Weeklyâ€). The graph we made could have been created in excel so lets jazz it up a bit.


# Animation

To start with let's animation which is super simple with transitions. You can animate multiple properties and even add effect like bounce. Here is an example of loading the graph using transitions. I refreshed it a couple of times in the video because the effect is so cool. [wpvideo ZbF9usve] In this case all that was added was a couple of lines describing what to animate (the x attribute) and what effect to use (bounce). <script src='https://gist.github.com/4638990.js'></script> The added commands are there on lines 9-11. Transition tells d3 to animated from the previous value of at attribute to the new value. In this example we haven't given any x value so the rectangles start off at the default x value of 0. Ease instructs d3 to use an effect, in this case the bounce effect. Finally duration tells 3d to make the animation take 750ms. Most properties can be animated. Here we have dropping and bouncing <script src='https://gist.github.com/4639018.js'></script> [wpvideo gjBv23aE] And this is my favorite: growing. In it you'll notice that I had to set up a default value for y and transition both y and the height. That's because 0,0 is in the top left and the bars would grow down, otherwise. <script src='https://gist.github.com/4639064.js'></script> [wpvideo R6FAvBoa]


# Interaction

Animation are all very well and are great for leveraging the [halo effect](http://en.wikipedia.org/wiki/Halo_effect)to ensure that people are enthusiastic about your application, but they aren't all that useful overall.Fortunately, d3.js defines the ability to add event listeners to your visualizationpermitting interaction. When I first played with them I used them to change the colours on bars as of the graph as I hovered over them. In his D3 book â€œInteractive Data Visualizationâ€ [Scott Murray](http://twitter.com/alignedleft)points out that this effect can be better created using only CSS' hover pseudo selector. That's unfortunate because up until I read that section it was going to be my example. Instead let's try adding extra information to the bar.

This ended up being way more complex than I had originally planned so let's build it up nice and slow. The first thing is that we add some additional information to each of the month bars. Here we've added weekly percentages to each month.

<script src='https://gist.github.com/4646705.js'></script>

We would like to divide the existing bars into bands when somebody mouses over them. To do this we can make use of the on() command. on takes two arguments, the first is the name of the event to bind, in most cases this will be mousover, mouseout or click. The second argument is a function to call when the event occurs.

<script src='https://gist.github.com/4646767.js'></script>#file-mouseover1-js-L9-L11

That's the easy part, the harder part is to come. We add to the current bar a number of additional bars

<script src='https://gist.github.com/4646809.js'></script>

On line 2 in this code we set up a new scale which generates a different colour for each entry. D3.js comes with a couple of built in colour scales and here we're using one with 10 colours. If this wasn't a demo script I would make my scale derivative of the original bar colour. Line 3 is just a shortcut to the currently covered bar. Line 4 gets the top of the currently selected bar, this will be where we start adding new bars. Line 5 is where things get interesting, you may notice it looks somewhat familiar. In fact we're using the same construct as earlier to define the bars. You'll notice this select-data-enter quite frequently in d3. The only complex attribute is the y attribute which changes with each element as each element must start further down the bar.

All of this gets use something which looks like

[wpvideo mtzMgPaN]

There is a obvious flaw in this in that moving the mouse off the chart doesn't remove the bars. To fix this we add a transparent rectangle over the top of the whole bar to detect when the mouse moves out. The original bar can't be used as it will be covered which will cause the mouse out event to fire erratically.

<script src='https://gist.github.com/4647153.js'></script>

Now it looks like

[wpvideo LobFZMpn]


# Conclusion

We've only scratched the surface of the cool visualizations which can be created with d3.js. HTML5 visualizations are a great way to help people understand data. There is so much information available in the world today that it is almost impossible to understand it with out some sort of a visual aide. I'm going to continue blogging about data visualizations as I learn more about d3. You should learn along with me!



