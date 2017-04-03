---
layout: post
title: What's this? - JavaScript Context
authorId: simon_timms
date: 2013-10-03
---

I have a little 2 year old son who's is constantly pointing at things and asking "What's this?". I'm a man of the world so I can almost always tell him what it is at which he's pointing. "That's a sink", "That's the front door", "That's an experimental faster than light quantum teleportation device I'm using to travel through time". "That's the huddled masses quivering in fear of my mastery over space time".

The one time I have trouble is when he points at a JavaScript function.

"What's this?"

Well, son, that depends. One of the weirdities of JavaScript is that the context in which a method is written is not, necessarily, the one in which it is run. Let's try that for a second. Here is some simple code which demonstrates two different ways to call a function

<script src='https://gist.github.com/stimms/6800466.js'></script>

The console here shows

<script src='https://gist.github.com/stimms/6800509.js'></script>

What is happening is that the first function is being called normally and the second is being called with a different context. In this case an evil context. The discussion of this oddity could end right here if nobody made use of call() or apply(). However people do make use of it, in fact jQuery makes very heavy use of it which, frankly, makes my life miserable.

Whenever you attach an action listener using jQuery the function runs in the context of the item to which the listener is attached.

<script src='https://gist.github.com/stimms/6800599.js'></script>

So the function here will be in the context of whatever.button-thing was clicked. I can see why this was done and it does make things very easy to code having the click context right there. However it makes for trouble when you're trying to follow proper namespaced JavaScript rules.Let's set up a demonstration here:

<script src='https://gist.github.com/stimms/6811086.js'></script>

If we run this, as shown at[http://jsfiddle.net/Aj2Sc/4/](http://jsfiddle.net/Aj2Sc/4/), then you can see we're not able to retrieve the value correctly because the context is not the same on in which listener is declared.

Fortunately there are workarounds for this sort of behaviour. The most common is to declare a variable which will hold a temporary copy of the correct this. Typically we call this variable one of {that, self, me} all of which are terrible names in my mind. We can replace the init function in our example with this one

<script src='https://gist.github.com/stimms/6811233.js'></script>

This rather interesting structure creates what is, in effect, a temporary context preserving class. It does this by passing the newly created self into a function which returns a function scoped to self. In effect it is a proxy.

If you think this hack is unappealing you're right. It is, unfortunately, the way that JavaScript works and you can't get around it. What you can do is pretty the whole thing up by using the jQuery function proxy like so:

<script src='https://gist.github.com/stimms/6811297.js'></script>

This function does all the same nastyness we did ourselves but at least it wraps it and makes the code readable.



