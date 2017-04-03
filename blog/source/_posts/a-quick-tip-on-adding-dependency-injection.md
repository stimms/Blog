---
layout: post
title: A Quick tip on adding dependency injection
authorId: simon_timms
date: 2014-04-14
---

I ran into the need today to move quite a number of classes into dependency injection. This can be a bit of a pain as you have to go through a ton of places to find where the class is used and get it out of the DI container instead of simply newing it up. See the constructor remains valid so you can still create an instance using just var b = new blah();

One trick I used which really helped speed up finding the places where the class was being manually created was to, temporarily, make the constructor private. This will cause all the places where the class is being instantiated to be highlighted as compiler errors. Once you're done fixing it then you can return the constructor to its previous state and go about your business. This is really just an application of the Lean on the Compiler pattern I first learned about in Michael Feathers' excellent book [Working Effectively with Legacy Code](http://www.amazon.ca/Working-Effectively-Legacy-Michael-Feathers/dp/0131177052). Well worth a read if you have any untested code to maintain.



