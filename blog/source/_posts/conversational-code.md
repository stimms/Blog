---
layout: post
title: Conversational Code
authorId: simon_timms
date: 2013-01-10
---

About a month ago Tom Janssens, who is a constant force on the DDD-CQRS mailing list and a generally smart dude, posted a link to his latest CQRS creation. Entitled [Mauritius](https://github.com/ToJans/mauritius) this CQRS example was an experiment around minimizing infrastructure. A lot of people get hung up on thinking they need a service bus and idempotent commands and the ability to handle asynchronous updates to their composite UI when they first look at CQRS. This isn't the case and you can make some very simple CQRS solutions like Mauritius and Greg Young's [Simple CQRS](https://github.com/gregoryyoung/m-r).

Anyway CQRS isn't the subject of this post. What I wanted to talk about what this great piece of code from Mauritius

<script src='https://gist.github.com/4507865.js'></script>

This is basically a conversational way of checking constraints. If you were to use it in, say, an ASP.net MVC controller it would look like

<script src='https://gist.github.com/4507884.js'></script>

Which is much easier to read and understand than the typical way of doing this

<script src='https://gist.github.com/4507896.js'></script>

I'd really like to see more of this style of coding, to me it exemplifies the idea of self documenting code.



