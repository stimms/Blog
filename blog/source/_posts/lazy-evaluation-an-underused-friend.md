---
layout: post
title: Lazy Evaluation -  An Underused Friend
authorId: simon_timms
date: 2013-01-08
---

In an informal code review today we came across some code which was nowhere near as performant as we needed. It was taking about 4 seconds and we needed it to be well under a second. After some print line debugging we found that the problem was in theapplicationof a chain of responsibility pattern.

The problem we were addressing was a series of token replacements. The value of each token could be provided by any one of half a dozen different token replacers.

<script src='https://gist.github.com/4490594.js'></script>

Each of the replacers basically looked like

<script src='https://gist.github.com/4490650.js'></script>

The problem with this is that the database is queried even if there is no token to be replaced by that particular replacer. I've been reading a bit about functional programming making a come back as of late so I thought I would take the opportunity to apply some of it here.

C# 3.0 introduced lambda functions which are, I would guess, mostly used for LINQ. In fact I would bet that 90% of programmers only use lambdas for LINQ. That's a shame because they're a very powerful tool. Really lambdas are just anonymous function which can be passed around as first class citizens. Using a lambda here coupled with lazy evaluation allows us to only perform the expensive database query when it is actually required. Thus all we need to do is update our code to make use of Lazy which is shockingly easy.

<script src='https://gist.github.com/4490742.js'></script>

In our case this improved our performance by about 90% and allowed me to lecture people about functional programming.



