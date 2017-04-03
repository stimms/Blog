---
layout: post
title: this vs. _this in TypeScript
authorId: simon_timms
date: 2013-01-28
---

One of the real difficult things to deal with in JavaScript is understanding exactly to what the variable â€œthisâ€ currentlyrefers. â€thisâ€ is a scope variable which means that it can change from line to line. In most languages this wouldn't be a big deal because the number of scopes is small but with JavaScript so much is done with anonymous functions that things become confusing quickly.

In TypeScript many of these internal function can be replaced with what I would call lambdas but I believe might also be known as â€œFat Arrow Functionsâ€. These are taken directly from ECMAScript 6.0. However there is a key difference between the new lambda functions and the current function denoted functions: the value of â€œthisâ€. In a fad arrow function the value of â€œthisâ€ is bound to the outer scope, the lexical scope.

So if you're at all familiar with d3.js which I've been using a lot as of late the â€œonâ€ function requires that â€œthisâ€ be permitted to be set by d3.

<script src='https://gist.github.com/4640450.js'></script>

TypeScript forces this to be bound to the outer context by replacing our call to this with one to _this which is a new variable that TypeScript creates. Obviously this doesn't work for our case as we expect this to be boud whatever d3 has found during selection.

There are a number of [possible fixes](http://stackoverflow.com/questions/12756423/is-there-an-alias-for-this-in-typescript) on StackOverflow but they seem over complicated to me and some of them are jQuery specific. Instead I recommend simply using a traditional function instead of the fat arrow function.

<script src='https://gist.github.com/4640466.js'></script>



