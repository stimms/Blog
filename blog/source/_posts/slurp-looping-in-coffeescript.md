---
layout: post
title: Slurp -  Looping CoffeeScript Part 2
authorId: simon_timms
date: 2013-04-22
---

There are a couple of different approaches to looping in CoffeeScript. The designers of CoffeeScript felt that the majority of loops are really just iterating over a collection. Thus the majority your looping needs can be served using a for-each style loop. The syntax for doing so looks about how you would expect

<script src='https://gist.github.com/stimms/5431462.js'></script>

Here we have a loop over the trees in theforest. Pretty easy. What if I really need to know the index of the element on which I was operating? Unlike for-each loops in many languages which assume that the index of and element in an enumeration in meaningless CoffeeScript offers a way out

<script src='https://gist.github.com/stimms/5431479.js'></script>

By passing in a secondparameterto the for loop you can gain access to an element which contains the index. If you don't have a collection but wish to execute some operation multiple times you can either use a for loop syntax on a range or use a while loop.

For loop with range:

<script src='https://gist.github.com/stimms/5431497.js'></script>

While loop:

<script src='https://gist.github.com/stimms/5431505.js'></script>

Perhaps the coolest feature of loops in CoffeeScript are that you can perform filtering on the elements as youiterateover them. If I only wanted to perform an action on my forest when the tree was birch then I can create an implicit if by doing

<script src='https://gist.github.com/stimms/5431535.js'></script>

Now let's say that the trees in the forest are a bit more complex that before and we wanted to filter on a property, that's no problem:

<script src='https://gist.github.com/stimms/5431549.js'></script>

Loops are much more powerful in CoffeeScript than in pure JavaScript. I think the syntax is also more readable.



