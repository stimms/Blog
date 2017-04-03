---
layout: post
title: Hyphenated Attributes in ASP.net MVC HTML Helpers
authorId: simon_timms
date: 2013-05-31
---

I had the need to emit an HTML attribute today which contained a hyphen. Aside: a hyphen is the thing which joins two words while a dash is the thing which is used to denote a sentence interruption

**Dash**

The zombies are coming "” I'm not afraid.

**Hyphen**

Zombies are flesh-eaters.

**Both**

The flesh-eating zombies are coming "” I'm not afraid.

There are two different dashes too, but we're already pretty far off track here "” let's reel it in.

There are actually an infinite number of valid HTML5 attributes which contain hyphens. You can write data-[anything] to add data to an arbitrary data to your elements. I find it to be very useful to have this standard for data attributes. If you're using AngularJS then you'll probably be making use of a lot of ng-* attributes. I came across some trouble when I was trying to emit this data from the HTML helper methods in ASP.net MVC. You can't just do

@Html.TextAreaFor(x=>x.Name, new { data-age="fifteen"})

Because the hyphen is interpreted as a minus. Solving this is actually very easy. Razor will automatically replace underscores with hyphens so

@Html.TextAreaFor(x=>x.Name, new { data_age="fifteen"}) gets you what you want.



