---
layout: post
title: Specific Types and Generic Collections
authorId: simon_timms
date: 2013-12-27
---

Generics are pretty nifty tools in statically typed languages. They allow for one container to contain specific types, in effect allowing the container to become an infinite number of specifically typed collections. You still get the strong type checking when manipulating the contents of a collection but don't have to bother creating specific collections for each type.

If you're working in a strongly typed language then having access to generic collections can be a huge time saver. Before generics were introduced in a lot of timewas spent casting the contents of collections to their correct type.

<script src='https://gist.github.com/stimms/8141339.js'></script>

Casting like this is error-prone and is the sort of thing which makes [Hungarian notation ](https://en.wikipedia.org/wiki/Hungarian_notation)look like a good idea. However generics are like most other concepts in programming: dangerous when overused.

C# provides a bunch of generic collections in System.Collections.Generic which are fantastic. However there are also some types which are ill advised. Tuple is one of the worst offenders

> Does anyone have a valid use case for a Tuple<T1,T2> where an explicit strongly-typed value object (even if generic) would not be better?
> 
> "” David Alpert (@davidalpert) [December 19, 2013](https://twitter.com/davidalpert/status/413723388473905152)

<script async="" charset="utf-8" src="//platform.twitter.com/widgets.js"></script>

There are actually 8 overloads of Tuple allowing for near endless combinations of types. The more arguments the worse the abuse of generics. The final overload of tuple hilariously has a final argument called "Rest" which allows for passing in an arbitrary number of values. As David Alpert suggested it is far better to build proper types in place of these generic tuples. Having proper classes better communicates the meaning of the code and gives you more maneuverability in the case of code changes. Tuples don't support naming each attribute so you have to guess what they are or go look up where the tuple was created. This is not maintainable at all and is of no help to other programmers who will be maintaining the code.

The C# team actually implemented Tuples as a convenience to help support F#'s use of tuples which is more reasonable. Eric Lippert suggests that you might want to group values together using a tuple when they are weakly related and have no business meaning. I think that's garbage. The idea that OO programming objects should somehow all map to real world objects is faulty in my mind. Certainly some should map to real world items but if you're building a system for a library then it pedantry to have a librarian class. The librarian is a construct of the actual business process which is checking out and returning books.

So that brings me to the first part of my rule on generics: use specific types for types instead of generic objects avoiding use of Tuple and similar generic classes. Building a class takes but a moment and even if it is just a container object it will at least have some context around it.

On the flip side generics are ideal for collections. I frequently run into libraries which have defined their own strongly typed collections. These collections are difficult to work with as they rarely implement IEnummerable or IQueryable. If new features are added to these interfaces such as with LINQ there is no automatic support for it in the legacy collections. It is also difficult to build the collections initially. For collections of arbitrary length make use of the generic collections, for collections of finite length use a custom class.

Generics are powerful but attention must be paid to proper development practices when using them.



