---
layout: post
title: Dart Part 2 -  Classes
authorId: simon_timms
date: 2013-04-29
---

Classes are a common structure in many programming languages. They are used to arrange code into logically related groupings. In object oriented programming the idea is that each class represents a thing. This thing may be a real world object like a Book or a thing which operates on a book like a BookSorter. Much like the other languages at which we've looked Dart has support for classes and also for fullinheritance.

<script src='https://gist.github.com/stimms/5477973.js'></script>

Here you can see Book has adescendantclass TextBook which adds the additional edition field. Just like CoffeeScript and TypeScript there issyntacticsugar to allow for setting of public properties from the constructor. In this case the â€œthisâ€ keyword us used to denote what should be set. Interestingly on line 18 we have access to the edition information even though the instance was created as a Book and not a TextBook. This would not be possible with a language like C# or Java.

Dart has aninterestingfeature which is a built in factory method. Factories can be used to construct objects in a non-initialized way. Basically it is a way of overriding construction. Honestly it is a useful pattern but it isn't one which I use so frequently that I feel the need to have my language support it. But perhaps you're different.

<script src='https://gist.github.com/stimms/5486061.js'></script>

In this example I retrieve the book from HTML5 local storage instead of creating a new book.



