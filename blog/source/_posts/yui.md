---
layout: post
title: YUI
authorId: simon_timms
date: 2013-05-08
---

I'm told that YUI is pronounced Y U I and not yoooii, which I feel is a realdisappointment. This post contains information gathered from[Jeff Pihach's](https://twitter.com/fromanegg) YUI talk at Prairie Dev Con. It is basically going to be a stream ofconsciousness with little ordering. Sorry.

- YUI has quite a bit of support from various companies. Yahoo is, obviously, a big user. That yahoo uses a technology is not really a selling feature for me. They don't even let people work from home.
- YUI loader does server sideconcatenationof modules to allow you to load dependencies into your YUI application. It is dependency aware so it will properly order your includes. There is support for minification and even for doing debug statement removal.
- Has support for a CSS selector based DOM selection and manipulation engine.

Y.one('.foo .bar'); //selects just one element which is returned as a Y.Node Y.all('.bazzs');//returns a node list which is an array with some extra prototypes on it

- Y.Node supports such methods as

setStyle() addClass() hide() show() ...

- There is binding for events which support binding before (using on), after (after) and instead of (delegate) events. Kind of handy.
- You can specify the context under which an event handler will run. Probably a bad idea.
- YUI is heavily modular so if you don't need gesture support or simulate DOM events just don't load those modules. Modules can be rolled up into â€œrollupsâ€
- YUI has some ability to create modules and classes for you. They have an augment function which is about the same as the jQuery .extend method. The classes look to have the ability to bind to change events on properties. So basically every object is an observable.
- There is a plugin model which implements the mixin pattern. Is mixin a pattern? Maybe it is a language feature.
- YUI base provides a simple basic object and pretty much everything extends that
- Y.Widget createsreusableGUI objects
- Y.Array.each is a handy foreach thing for arrays.
- There appears to be an extension method which allows for overriding 3rd party controls through monkey patching
- Y.App is a rollup which provides for an MVC style framework. Has built in support for routing.
- ModelSyncRest is a framework which allows for making changes to a bunch of data using REST. It is very quick to set up CRUD sort of pages using a REST backend

Thoughts:

I'm not in love with having different selectors for a list of elements and a single node. That is one of the great selling features of JQuery. Having <del>two</del>three different object extensibility methods is a bit complicated. I've never been convinced by mixins so I'm a bit one sided on that.

Why is enumerating an array so complicated, huh JavaScript? Old JavaScript engines are the bane of the Internet.

I think that YUI has some potential but that the bar to entry is pretty high. I'm not fully convinced that the advantages are so big that they cover the cost of dealing with the cost. I find myself still asking Y.Why? (I spent half the talk thinking up that joke)



