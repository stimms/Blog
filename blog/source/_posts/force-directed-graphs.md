---
layout: post
title: Force Directed Graphs
authorId: simon_timms
date: 2013-02-28
---

In talking with a friend the other day he mentioned that everybody at his work was all agog about a TED talk which had a cool looking graph in it. He promised that he wouldabsolutely100% pinky swear send me a link to the talk so I could try to recreate it. He didn't.

I had a pretty good idea what it was he was talking about though: a force directed graph. The idea behind a force directed graph is that you have a number of connected nodes which are attached using springs and attracted by gravity. These graphs can be used to show relationships between a number of items and they are interactive so that they can be dragged around to see what the data would look like from a different direction. The proximity of nodes to one another can denote the strength of the relationship.

Let's try to recreate it using our good friend d3.js. The first thing we need is a set of related data. Wikipedia is a great source for data of this sort of data. A good data set for a demonstration will have nodes which are connected to more than one other node and may have another aspect to it like that some of the nodes share another property. This additional degree of relationship can be denoted with colour.

I took a look at a few pages of data but I'm a nerd so I chose the dataset of actors with whom [Joss Whedon](http://en.wikipedia.org/wiki/Joss_Whedon) has collaborated. If you just clicked on the link to see who Joss Whedon is the you get off this blog, you get off and you never come back.

I started by pulling the table from wikipedia and then transforming it into JSON. I got some help in doing that from[http://jsonlint.com/](http://jsonlint.com/)which is a great tool for checking and formatting JSON. The file is pretty long but a chunk of it looks like

<script src='https://gist.github.com/stimms/5061322.js'></script>

You may notice that I included the names of the productions and their medium. We'll see more about this tomorrow when we add filtering to the graph.

Fortunately for us d3 provides some helpers to set up a force graph. I basically stole my entire graph code from Mike Bostock's [page](http://bl.ocks.org/mbostock/4062045). d3 requires that you set up a list of nodes and edges.

Nodes are quite easily set up and are just represented as circles. This is pretty much what we've seen before except that we call force.drag which, if you drill into the example you'll see allows for moving the nodes

<script src='https://gist.github.com/stimms/5061606.js'></script>

The edges have different strengths, the higher the value of the links the stronger the connection so the closer the nodes would be. I built the links based on the productions shared between the two people. The code for extracting the shared productions is"¦ umm"¦ not pretty. I really don't know how you would make it prettier other than changing the underlying data structure. So I guess the lesson here is: pick data structures which work with your requirements.

<script src='https://gist.github.com/stimms/5061621.js'></script>

The resulting graph looks like this:

<div class="wp-caption aligncenter" id="attachment_2384" style="width: 302px">[![Graph](http://stimms.files.wordpress.com/2013/03/screen-shot-2013-02-28-at-5-58-03-pm.jpg?w=292)](http://stimms.files.wordpress.com/2013/03/screen-shot-2013-02-28-at-5-58-03-pm.jpg)Graph

</div>If you want to see the interactive version you should pop over to [http://bl.ocks.org/stimms/raw/5061669/](http://bl.ocks.org/stimms/raw/5061669/)



