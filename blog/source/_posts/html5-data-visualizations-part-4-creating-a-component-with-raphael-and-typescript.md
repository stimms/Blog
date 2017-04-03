---
layout: post
title: HTML5 Data Visualizations "“ Part 4 "“ Creating a component with RaphaÃ«l and TypeScript
authorId: simon_timms
date: 2013-01-16
---

<span style="background-color:lightyellow;border-color:#E6DB55;border:solid 1px;display:block;">**Note**: I will be presenting a talk on data visualization in HTML5 on February the 14th at the Calgary .net user group. Keep an eye on[http://www.dotnetcalgary.com/](http://www.dotnetcalgary.com/) for details. This blog is one in a series which will make up the talk I will be giving.</span>

In the last article we created a simple little hard coded graph with RaphaÃ«l. I also insulted the Germans. That wasn'tentirelyfair because RaphaÃ«l is really an Italian name and we never thrashed them in a war, probably because they were too busy being defeated by the French. The French. Oh, and we did thrash the Italians too. That's why you come to this blog: friendly racism, code and an inability to forget wars which happened 40 years before the author was born. So we're' still going to call the library [Ralph](http://raphaeljs.com/).

Building JavaScript graphs is pretty easy with Ralph but we still don't want to recreate the graph on every page where we need a graph. The solution is to turn it into a component. Honestly JavaScript is pretty crummy at being put into a component. Creating classes and namespaces in javascript is possible but it is a lot of boiler plate code and look like [a mess](http://elegantcode.com/2011/01/26/basic-javascript-part-8-namespaces/). Keeping code in namespaces or modules is key to building a scalable application where there are no collisions between names.

Fortunatelywe live in an age where there are multiple languages which transcompile down to JavaScript. For this effort we're going to make use of TypeScript. TypeScript is an open source project which was created by Microsoft. It makes use of some conventions which are looking like they are going to be a part of the next release of JavaScript. In many ways it is like JavaScript vNext right now.

I'm going to dive right in and rewrite this puppy as a component in TypeScript

<script src='https://gist.github.com/4554073.js'></script>

Okay that was fun but what is all this doing? Well on line one we have started by creating a module. A module is basically a namespace. Then on line 2 a public, or exported class inside that module. This is just like you would expect a class to behave in Java or C#. Within that class we define a constructor. One of the cool things is that prefacing each parameter in the constructor with public make it a public property on the class automatically and assigns the value from the constructor to it. C# could use a shortcut like that! We also set the data types. This doesn't really do anything with the generated JavaScript but it does throw compiler errors if we use a string when a number is expected. TypeScript is great in that it allows you to tune how much strong typing you want.

Another great feature in the component is the use of ES5 style loops on line 29. This is basically an iterator syntax.

The final thing to which I wanted to draw your attention was the lambda style anonymous functions. You can see this inside the same loop on line 29. I've always found this syntax cleaner than a full function. The => is sort of the official syntax now for lambda functions with C# and Java both adopting it.

To make use of our new graph component is easy. We just call

<script src='https://gist.github.com/4554116.js'></script>

Now, of course, this all compiles down to JavaScript because you could have go exactly the same functionality in pure JavaScript. What does that look like? Well it is a bit more verbose:

<script src='https://gist.github.com/4554120.js'></script>

I find the TypeScript to be way more readable.

In the next part we're going to look at an alternative library which might be better suited for creating data visualizations.



