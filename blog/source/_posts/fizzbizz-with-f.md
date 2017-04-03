---
layout: post
title: FizzBizz with F#
authorId: simon_timms
date: 2013-03-08
---

A while back [I blogged about](http://blog.simontimms.com/2013/01/30/fizz-buzz-returns/ "Fizz-BuzzReturns") how I thought having a FizzBizz like problem on a job interview was a good idea. In that post I mentioned that coming up with novel ways to do fizz bizz should be fun for more senior developers. I though perhaps it might be fun to try out this F# language I've been hearing so much about as of late. So I poped over to the [Try F#](http://www.tryfsharp.org/Learn/getting-started) site where they have a nifty online F# interpretor. A bit of paying resulted in

<script src='https://gist.github.com/stimms/5117080.js'></script>

F# is a functional language which has really good support for lists or arrays. Here I used the | operator which is a forward-chaining operator to pass the results from one function to the next. It is reminiscent of shell scripting (incidentally doing fizz bizz in shell scripting would also be fun). As part of F# there is a concept called a filter which acts in sort of the same way as a switch statement. It allows you to match the elements of a list and perform different actions depending on the match. That's what you see on lines 3-6.

Being pretty new at this F# stuff I went hunting for other people's solutions online. No better way to learn a language than to see how other people use it. That's why github is so awesome. Well it is one of the reasons. I found a swell answer on StackOverflow in[Tomas Petricek](http://stackoverflow.com/users/33518/tomas-petricek)â€˜s [answer](http://stackoverflow.com/a/2429874/361). The gist of it seems to be that this is a functional language and we should be attempting to use it functionally. To do that we can declare an active pattern and slot that in instead of the individual modulo checks. This allows us to remove the where from the filter

<script src='https://gist.github.com/stimms/5117219.js'></script>

So there is an F# version of FizzBizz.



