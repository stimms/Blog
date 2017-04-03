---
layout: post
title: Publish All the Code
authorId: simon_timms
date: 2013-01-03
---

I don't know about everybody else but when I publish code on github or even in a mailing list post I try extra hard to make it not suck. Of course it still sucks but at least I know that it's because I'm notcompetentand not because I'm lazy. Putting code out there for others to read forces you to think just that little bit more about how good it is and, hopefully, make it a bit better.

The extra though is one of the reasons that I'm a big advocate of code reviews. Lately I've been thinking a lot about coupling code reviews inside my group with code reviews from outside my group. The culture in the rest of the company I'm with is not really conducive to having other people review my code but that's never stopped me before. We have a github organization and whenever people areinterestedin our code I grant them read access and tell them we accept pull requests. Typically this gets the old squint of confusion. Even afterexplanations the squint usually remains.

The problem is that with most large companies the idea that people who don't work directly on the product can see the code is totally foreign. If you ask around nobody is really sure why that is so they throw out the excuse of security.

> If people can see the code then they might be able to break into the system or they might steal the code and give it away.

The first part of this argument is a terrible one. Security through obscurity [has](http://www.schneier.com/essay-056.html) [never](http://www.schneier.com/crypto-gram-0205.html#1) [worked](http://en.wikipedia.org/wiki/Kerckhoffs%27_principle)very well. The second part is slightly better but if you don't trust the people you have working for you whey are they working for you? There have been code leaks in the past but I have been unable to find a notable case where the leak was caused by an employee leaking the code.

What I'm proposing is that software development of internal tools in a company should be open within the company. There are a number of tools I use which could be improved with a few minor changes. The developers of these tools are either too busy or too out of touch with their user base to make the changes themselves. If these tools were internally open sourced then I could have submitted a pull request to the team â€“ saving their time and improving the product. If everybody should [learn to code](http://venturebeat.com/2012/09/17/why-everyone-should-code/)then the obviouscorollaryis that all code should be open.



