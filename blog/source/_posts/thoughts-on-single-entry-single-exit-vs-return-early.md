---
layout: post
title: Thoughts on Single Entry-Single Exit vs. Return Early
authorId: simon_timms
date: 2013-03-07
---

There are two schools of thought behind how to construct methods. The first says that your method has one entry point so it should have one exit point. A method like that would look like

<script src='https://gist.github.com/stimms/5113628.js'></script>

With this it is easy to see where the value is returned to the caller. Other than that there is not much good about the structure of the code. There are a lot of brackets and you have to read the whole function to figure out what it is doing. As I understand it this particular structure of code is a throwback to the days of structured coding and was used to avoid the dreaded GOTO. The other method of designing a method is to return early. If we rebuild that last method it would look like

<script src='https://gist.github.com/stimms/5113984.js'></script>

Here we can see that there are a bunch of places which return. As we read through it is easy to see where we return. There is no need to read the entire function as we trace through it. At the top is a guard clause which will return right away if the parameters to the function are incorrect.

Now to be fair I took returning early to an extreme in this example. Everywhere I could retur early I did. If I were to come across this function in a code review it would be questioned. I would probably rewrite it to use a number of functions, something like

<script src='https://gist.github.com/stimms/5114071.js'></script>

It is still a bit messy but that is probably more a function of a poor API for the imaginary client I created.

So the conclusion is that single entry-single entry is an outdated concept which makes code difficult to read. At the same time leaning heavily of return early createsmessycode too. Mixing the two and makingintelligentdecisions on a per method basis is the optimal strategy.



