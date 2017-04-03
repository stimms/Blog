---
layout: post
title: Dart Part 4 -  DOM Changes
authorId: simon_timms
date: 2013-05-01
---

It is tough when looking at Dart to remember that compiling Dart to JavaScript isn't the end game of Dart. This is a much bigger project than CoffeeScript or TypeScript which just aim to make writing JavaScript less painful. Dart is on a quest tocompletelychange how web browser scripting is done. So when I compare dart:html to Sizzle it is a false analogy. Sizzle is written in JavaScript and talk to the browser through a set of APIs which have grown up over the last decade or so.

[![dart](http://stimms.files.wordpress.com/2013/05/dart.jpg)](http://stimms.files.wordpress.com/2013/05/dart.jpg)

The assertion of the Dart folks over at Google is that the API which has grown organically is inconsistent and unmaintainable. Based on some of their examples I can't really disagree. Let's take a look at a couple of them.

Say you want to query for some information out of the DOM. There is a rich JavaScript API for that. Why you just need to pick one of these methods

getElementsById() getElementsByTagName() getElementsByName() getElementsByClassName() querySelector() querySelectorAll() document.links document.images document.forms document.scripts formElement.elements selectElement.options

That's pretty clear. In Dart your choices are

query() queryAll()

That's it. All the power of selecting items is still there but it now hidden in the parameters passed to these two functions. The parameters are written in the same DSL as CSS so it is something that web developers already know. Vendor prefixes are another big issue in modern development. When browser vendors create newfunctionalitythey hide it behind a vendor prefix until it is ready for general consumption. It's not a big deal if you're on a rapidly updating browser like FireFox or Chrome but if you're on IE with yearly release cycles the it is tedious to deal with.

If you want to get access to the camera from a browser in JavaScript you're going to do

navigator.getMedia = ( navigator.getUserMedia || navigator.webkitGetUserMedia || navigator.mozGetUserMedia || navigator.msGetUserMedia);

Dart abstracts away all those silly vendor prefixes and give you

window.navigator.getUserMedia(audio:true, video: true)

Way nicer! You can still have all the nice syntactic sugar of jQuery but now jQuery itself can be much smaller and easier to maintain because it doesn't have to smooth out the weirdbehaviorsbetween browsers.

We need this. I don't know if Dart is the solution but we need a clean break from maintaining the garbage which is the interface between JavaScript and the browser DOM. I'm glad that somebody is at least making an effort.



