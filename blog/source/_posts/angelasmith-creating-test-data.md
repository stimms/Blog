---
layout: post
title: AngelaSmith -  Creating Test Data
authorId: simon_timms
date: 2013-02-26
---

Yesterday I blogged about a rapid data prototyping tool called AngelaSmith which permits building realistic test data. Today I'm going to show you how to make some use of it. The first thing we need is a model on which to operate. Ever since I read the license agreement for Java 1.1 which explicitly prohibited the development of software related to airplanes I've liked example code which references airplanes. I even had a text book at one point in which all the examples were related to the flying of planes. I can't remember which book it was but it might have been an old [Deitel](http://www.amazon.com/exec/obidos/ASIN/0131016210/deitelassociatin)book.

Anyway let's start with this model:

<script src='https://gist.github.com/stimms/5035844.js'></script>

This can be filled with AngelaSmith

<script src='https://gist.github.com/stimms/5035900.js'></script>

But if we print out the data we can see that Angela wasn't quite smart enough to look at substrings of the field names to perform matches. That's a shame and probably going to be a pull request.

<div class="wp-caption aligncenter" id="attachment_2368" style="width: 760px">[![Not great test data](http://stimms.files.wordpress.com/2013/02/screen-shot-2013-02-25-at-9-37-07-pm.jpg)](http://stimms.files.wordpress.com/2013/02/screen-shot-2013-02-25-at-9-37-07-pm.jpg)Not great test data

</div>I did have a good laugh at the idea of a pitchfork with a range of 58km. That's a long pitchfork!

So we're going to have to set up some custom rules for the data generation

<script src='https://gist.github.com/stimms/5036109.js'></script>

For the most part we can lean on Jen(oh such witty and confusing naming) to do the population for us, but you can see that I used my own custom generation method for flight numbers because they're not really standard.

<div class="wp-caption aligncenter" id="attachment_2369" style="width: 760px">[![Better generation](http://stimms.files.wordpress.com/2013/02/screen-shot-2013-02-25-at-10-26-23-pm.jpg)](http://stimms.files.wordpress.com/2013/02/screen-shot-2013-02-25-at-10-26-23-pm.jpg)Better generation

</div>Well that was pretty easy! Thanks, Angela, you're the best.



