---
layout: post
title: Dart Part 1 -  Another New Hope
authorId: simon_timms
date: 2013-04-26
---

In preparation for my upcoming talk about next generation JavaScript I've been looking atalternativesto writing pure JavaScript. Largely my discussion is predicated on the idea that JavaScript isn't great for building large applications. As others have suggested JavaScript is the assembly language of the web. So far I've looked at TypeScript and CoffeeScirpt, today bring Dart into the mix.

<div class="wp-caption aligncenter" id="attachment_2654" style="width: 760px">[![Wraith Darts - sorry, Google, these are still cooler](http://stimms.files.wordpress.com/2013/04/darts.jpg?w=750)](http://stimms.files.wordpress.com/2013/04/darts.jpg)Wraith Darts "“ sorry, Google, these are still cooler

</div>Dart is a product of Google and having been announced in October of 2011 it is fairly mature. When it first came out the compiler for it was terrible and produced the most verbose JavaScript imaginable. It was unfortunate because it was soundly mocked for being junk. I seem to remember that a hello world application compiled to over 1000 lines of JavaScript code. Things are greatly improved now and Google recently announced that the JavaScript emitted by the latest compiler, dart2js, runs faster than hand written JavaScript. That's quite an achievement considering the crazy lengths to which browsers go to optimize JavaScript.

What's interesting about Dart, which makes it different from the other two languages we've looked at, is that you can actually run dartnativelyon Chrome. There is no need to compile it to JavaScript first. Well no need assuming that everybody who uses your site runs a special build of Chromium. However, I hear that the new Blink rendering engine is going to include support for Dart so there may be a future for native Dart and that future could be very close.

This post's goal is just to get started with the Dart development tools. Google provide a Dart editor which is based on Eclipse(ugh). So let's ignore that for now and use the command line tools. Grabbing the install package from[http://www.dartlang.org/tools/](http://www.dartlang.org/tools/)gives us the editor and the SDK. Within the SDK are a number of tools but we're interested in dart2js.

It can be run without any flags to produce both the compiled JavaScript and a map file. A real basic bit of Dart like this

<script src='https://gist.github.com/stimms/5471921.js'></script>

prints

0 Hello 1 Hello 2 Hello 3 Hello

The code demonstrates some small part of Dart but nothing too exciting. We'll get more into Dart in upcoming days.



