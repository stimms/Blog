---
layout: post
title: Dart Part 3 -  Libraries
authorId: simon_timms
date: 2013-04-30
---

Unlike CoffeeScript and TypeScript Dart is not really meant to be compiled to JavaScript. You certainly can compile it to JavaScript but the long term goal of the language is to replace JavaScript. That's a goal which is at least a decade out. Truth be told I don't see Dart ever replacing JavaScript wholesale. Selling Dart to Microsoft and to Mozilla is going to be quite a thing when there is already such acceptance of JavaScript. JavaScript hangs around rank 9-11 in the language ranking while Dart is not even in the top 50. That's not to say that Dart isn't a fantastic language it is just new and facing a pretty entrenchedcompetitor.

So being a full replacement for JavaScript Dart must alsofulfilthe role of many of the fantastic libraries which have become commonplace in the land of JavaScript. Dart includes a number of libraries for manipulating html, performing asynchronous actions, runningmathematicaloperations and even fir file I/O. These library create a sort of base class library the same as what you get with Java and .net. Obviously they are too large to cover in a single post. I thought I might talk a bit about dart:html which is the library designed for manipulating andqueryingHTML.

The first thing in the html library is a sizzle like selector engine which works on CSS3 selectors. It is easily usable via the query and queryAll functions. Query returns an Element and queryAll returns a list of elemnts. These elements can be manipulated just as you would in JavaScript. For instance this code will add a colour a couple of divs

<script src='https://gist.github.com/stimms/5492119.js'></script>

On the trivial html test page I put together the result was

<div class="wp-caption aligncenter" id="attachment_2664" style="width: 111px">[![Coloured boxes thanks to dart](http://stimms.files.wordpress.com/2013/04/boxes.jpg)](http://stimms.files.wordpress.com/2013/04/boxes.jpg)Coloured boxes thanks to dart

</div>Exciting!

If you look back at [part 1 of this series](http://blog.simontimms.com/2013/04/27/dart-part-1-another-new-hope/ "Dart Part 1: Another NewHope") I mentioned that there was some concert over the amount of crud which was created by Dart during a normal compile. To make these boxes dart2js generated 1891 lines of code. Hummâ€¦ that seemsexcessive. But it isn't really a fair comparison is it? I included dart:html which is likely huge as it contains a bunch of the functionality of jQuery. If you read the Dart documentation it suggests that Dart will actually trim out functions which are not used. There is also a â€“minify flag which can be passed to the dart2js compiler. Using the minify option the library is shrunk way down and the results are in line with other selector libraries.

<table><thead><tr><th>Library</th><th>Size when Minified</th></tr></thead><thead><tr><td>Sizzle</td><td>17KB</td></tr><tr><td>jQuery</td><td>32KB</td></tr><tr><td>dart:html</td><td>25KB</td></tr></thead></table>It is difficult to say if comparing dart:html to Sizzle or jQuery is more accurate. There is more that just a selector engine in there but there is also less that full blown jQuery. There is some interesting stuff built into dart:html and its companion libraries. With dart:uri and dart:async we pick up support for web sockets as well as Ajax. I don't feel like the syntax is all there yet but it does have the advantage of being built into the language. Well built into the base libraries. Most times developers are more likely to use the built in libraries instead of findingthirdparty libraries so there is probably more consistency moving from one Dart project to another than moving from one JavaScript project to another. The libraries aren't terrible but they aren't fantastic either.

â€œNot terrible but not fantasticâ€ pretty much sums up my feelings about Dart in general.



