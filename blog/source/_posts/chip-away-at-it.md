---
layout: post
title: Chip Away at It
authorId: simon_timms
date: 2013-03-14
---

Sometimes I go to a bootcamp style gym and theprescribedworkout is something insane like [100 pullups, 200 pushup and 300 squats](http://www.crossfit.com/mt-archive2/000881.html). On the surface this seems impossible. On a good day I can do perhaps 20 pullups in a row which is only 20% of the number needed here. During the workout, which took me a little over an hour last time the muscle rich coaches yell platitudes like something from a [Simpsons episode](http://www.youtube.com/watch?v=R4i8SpNgzA4&t=0m47s). One of the favorite chants is "Chip away at it, chip away".

With ripped hands and believing that I would never again be able to inhale like a normal person again I have very little interest in hearing the chants. (My favorite one of the last week was "burpies should be your rest, they're easy") However the chip away comment I find to be pretty applicable to software development. On any project of any size and of any age there is going to be a great deal of technical debt. Trying to pay down technical debt all at once is impossible. You'll never convince management that all new development needs to stop so that you can make invisible improvements to the code base. Instead you should make as small a changes as possible while you're doing other development.

My rule is that if I've opened a file, even if I'm just reading code, then I need to make an improvement. It could be as simple as removing unused using statements or changing a stringconcatenationto a string.format. These don't seem like they're going to do much to pay down your technical debt but their effects arecumulative. Eventually you run out of trivial changes to make in your files and you start making slightly larger changes. These slightly larger changes add up to larger changes. Before you know it your code base has been improved.

I followed this idea at a job once. When I started it took us 2 months to do a realease of our software. We identified the biggest pain point and automated it. Then we repeated this process until we actually ran out of things to automate. It took a couple of years but we got builds down so that they were running every night and producing full release packages. I automated myself out of a job. At least I would have if we didn't end up responsible for two dozen additional products. If we hadn't worked away at it wewouldhave been working 90 hour weeks just to stay on top of things.

Paying down technical debt is like paying a mortgage. If you just pay a little bit more each week then your debt will be paid down much sooner.

Chip away at it.



