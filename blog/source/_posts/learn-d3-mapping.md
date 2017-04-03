---
layout: post
title: Book Review -  Learn D3.js Mapping
authorId: simon_timms
date: 2015-02-23
---
I'm reviewing the book "Learn D3.js Mapping" by Thomas Newton and Oscar Villarreal.

```
Disclaimer: While I didn't receive any compensation for reviewing this book I did get a free digital review copy. So I guess if being paid in books is going to sway my opinion take that into account.
```

The book starts with an introduction to running a web server to play host for our visualizations. For this they have chosen to use node which is an excellent choice for this sort of lightweight usage. The authors do fall into the trap of thinking that npm stands for something, honestly it should stand for Node Package Manager.

The first chapter also introduces the development tools in the browser. 

Chapter 2 is an introduction to the SVG format including the graphical primitives and the coordinate system. The ability to style elements via CSS is also explored. One of the really nice things is that the format for drawing paths, which is always somewhat confusing, are covered. Curved lines are even explored. The complexity of curved lines acts as a great introduction to using the mapping functionality in d3 as it acts as an abstraction over top of the complexity of wavy lines. 

In chapter 3 we finally run into d3.js. The enter, exit and update functions, which are key to using d3 are introduced. The explanation is great! These are such important things and difficult to explain to fist time users of d3. Finally the chapter talks about how to retrieve data for the visualization from a remote data source using ajax. 

In chapter 4 we get down to business. The first thing we see is a couple of different projections available within d3. I can't read about Mercator projections without thinking about the map episode of the West Wing. That it isn't referenced here is, I think, a serious flaw in the book. Once a basic map has been created we move onto creating bounding boxes, choropleths(that's a map with colours representing some dimension of data) and adding interaction through click handlers. No D3 visualization is complete without some nifty looking transitions and the penultimate section of this chapter satisfies that need. Finally we learn how to add points of interest. 

Chapter 5 continues to highlight transition capabilities of D3. This includes a great introduction to zooming and panning the map through the use of panning and zooming behaviours. The chapter then moves onto changing up projections to actually show a globe instead of a two dimensional map. The map even spins! A great example and nifty to see in action. 

The GeoJSON and TropoJSON file formats are explained in chapter 6. In addition the chapter explores how to simplify map data. This is actually very important to get any sort of reasonably sized map on the internet. At issue is that today's cartographers are really good and maps tend to have far more detail than we would ever need in a visualization. 

The book finishes off with a discussion of how to go about testing visualizations and JavaScript in general. 

This is an excellent coverage of a quite complex topic: mapping using D3. I would certainly recommend that if you have some mapping to do using D3 that purchasing this book might save you a whole lot of headaches. 
