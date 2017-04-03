---
layout: post
title: Strictly JavaScript
authorId: simon_timms
date: 2013-05-03
---

ECMAScript or JavaScript as it is more commonly known has gone through a number of large iterations over the years. The version currently under development is known as ECMAScript 6 or Harmony, which is an awesome code name. However most JavaScript we see around is actually ECMAScript 3. ECMAScript 4 was a failedendeavour but ECMAScript 5 brings a great new feature which we, as developers, should be exploiting: Strict Mode.

Back in the day Visual Basic had a feature called Option Explicit which was recommended. It required that you actually declare all your variables. Adding a simple command to your JavaScript enables strict mode for the whole file

"use strict";

What does this strict mode do? A bunch of great things. The first is that it does the same thing as Option Explicit in VB: it requires that all your variables be explicitly defined using var. Without strict code such as

monkey = "salmon"

will attach a monkey property to the global object, in most cases window. This is a dangerous behaviour as it allows information to leak from one function to another. Strict will catch that and throw an error. Instead you need to do

var monkey = "salmon"

This alone should be enough reason to use strict mode. But there is more! Way more.

Duplicate property assignment is detected and prevented so you will know right away if you do

var shoe = { size: 7, size: 9}

you're also prevented from putting in duplicate parameters into a function declaration

function climbTree(treeName, height, treeName) { }

Finally strict mode restricts the use of the eval key-word. You can no longer use it as a parameter or as the name of a function or even as a variable. It also prevents variable assignment from inside an eval context so you can no longer do

eval("importantOuterVariable = 'delete everything';");

Although I found that variable assignment from eval was still permitted in chrome.

Adding strict to any JavaScript is likely to catch errors before they become bugs in the real world. I think it is highly advisable and so easy to do. So why don't you get a little stricter with your JavaScript?



