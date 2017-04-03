---
layout: post
title: Does var compile to the same code?
authorId: simon_timms
date: 2013-01-23
---

In a code review the other day the topic of when to use var and when to use a non-inferreddata-type. That's a religious argument and probably a post for another day but the question of

> Is there any difference between the code produced using var and using, say, string?

I confidently answered â€œNo, it is identical. The type is inferred by the compiler and replacedâ€. But I was thinking about it later and I wondered if I was right.

I created a really simple test program

<script src='https://gist.github.com/4582521.js'></script>

This code compiled down to

<script src='https://gist.github.com/4582508.js'></script>

Changing the string to var produced exactly the same IL. So, oddly, I was right about something.



