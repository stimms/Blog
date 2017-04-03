---
layout: post
title: Slurp Deconstruction CoffeeScript Part 4
authorId: simon_timms
date: 2013-04-24
---

Welcome to part 4! This is the first part which is divisible by 2 but isn't equal to 2. Of course that wouldn't be true if I started my blog series at 0. Son of a gun â€“ you just wait for the next series.

Deconstruction are one of my favorite features of CoffeeScript. Deconstructions allow for rapid assignment of variables or rapid remapping on complex objects. The simplest case is the old interview question of how to swap two variables.

<script src='https://gist.github.com/stimms/5449166.js'></script>

This isn't limited to only two items either, you can do anarbitrarynumber

<script src='https://gist.github.com/stimms/5449169.js'></script>

Nifty but not all that useful, right? Not so fast my part 4 friend. Using this technique we can easily supportmultiplereturns from a function which is really the dream. Frequently you want to return complex data from a function but don't want to waste time building a bunch of data transfer objects

<script src='https://gist.github.com/stimms/5449214.js'></script>

Just. Like. That.

It can also be used to expand object like the options hash which is commonly passed in when building jQuery plugins.

<script src='https://gist.github.com/stimms/5449236.js'></script>

See? That's why part 4 is all about the fun of deconstructions.



