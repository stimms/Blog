---
layout: post
title: Primitives Hate You
authorId: simon_timms
date: 2013-01-14
---

Primitive types are a lot like cows: they seem friendly but if they got the chance they would[eat you and your entire family](http://www.youtube.com/watch?v=2pJlgz8l40A&t=21s). With a little bit of effort, though, you can defuse the problem. This has largely been done by people who practice Domain Driven Design(DDD) for some time. One of the tenants of DDD is that of value objects. The concept is a little difficult to grasp but basically it comes down to objects which are equal when their value is equal.

Here's an example: I once worked in a training institute where we had a huge number of student records. Because the software which ran the registration department wasn't very good the registration staff tended to create new records for every student who phoned up to register. This meant that we had literally thousands of Bill Smiths in the system. Each one of these had a different student number(which was used as the Id) but their names and addresses could often be the same. We once tried to merge these records together but it proved to be a total disaster. You would think that two Bill Smiths who live at the same address would most likely be the same person, however we found that the number of people who name their kids with their name is astronomically high. These two Bills Smiths were father and son so merging them didn't work. Students are an example of an entity. When their Ids are different then they are different. However each one of these entities had a value object of an address in them. If the properties of two addresses are the same then the addresses are the same and can be swapped one for the other.

Encapsulating the various fields of an address into a value object allow you to be sure about what you're dealing with and handle it correctly. F# applies this idea to numbers and allows you to include[units in the definition of numbers](http://blogs.msdn.com/b/andrewkennedy/archive/2008/08/29/units-of-measure-in-f-part-one-introducing-units.aspx). This is the sort of thing which could have save NASA from[crashing a probe into Mars](http://www.wired.com/science/discoveries/news/1999/09/31631). But it isn't just numbers you can apply it to, you can use the idea everywhere. I often see code which looks like

<script src='https://gist.github.com/4535764.js'></script>

This is a quick utility method for determining the largest file in a directory. On the surface it is an easy function and the parameters look good. However is there anything here which stops me from passing in a directory name of â€œmy uncle is a small fishing town on the coast of the Adriaticâ€? Not really. The problem is that passing in a string doesn't give any hints as to what is expected by the function. Sure the name of theparameterhelps but can I pass in relative directory names? How about directories on a file share, is that permitted? The return type seems good too. But the string which is passed back could be a local file name or a full file name, it is impossible to tell without dropping to the documentation which may, or may not exist. The function has a looser contract than it could have.

Noted brain-box[Amir Barylko](http://www.twitter.com/abarylko)once taught me to have functions take the most limited possible object. Don't let functions take in a more powerful object than they need. So if your function doesn't need to update the properties in a list have it take an IEnumerable instead. To that I would like to add â€œbe explicit about the datayou're consuming and producingâ€.

<script src='https://gist.github.com/4535811.js'></script>

Here all the confusion about what sort of information is going into the function and coming out has been eliminated byencapsulatingtheprimitivesin a value object*. Much cleaner, don't you think?

*Yes, I know that strings aren't primitives in C#. Feel free to substitute the string for an integer in your picky minds. I'm talking more here about using generic types than what is officially a primitive.



