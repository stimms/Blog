---
layout: post
title: Typescript - Creating large programs
authorId: simon_timms
date: 2013-03-05
---

Ten years ago when I wrote JavaScript I only ever had one function and the function was always called validate. The only function for which I used JavaScript was validating for entry. Somewhere along the line JavaScript became awesome and it was possible to build large applications using JavaScript. I personally think the watershed moment was when gmail burst onto the scene. I understand that the first version of gmail waswrittenin Java using aframeworkcalled GWT which transcompiles to JavaScript. However, the point remains that it is possible to build complex applications using JavaScript.

In object oriented languages code is arranged into functions, these functions are grouped together into classes and the classes are placed into namespaces. This hierarchical approach allows for thearrangementof large quantities of code into a logical structure. For the most part this method of arranging code has gone unchallenged for decades(with the exception of AOP, but that's a whole other post). JavaScript was never envisioned as a language for building large applications so it lacks first class support both classes and namespaces. Just as with Perl it is possible to simulate objects using arrays and even namespaces.Unfortunately,the syntax is prettyarcane.

Typescript to the rescue!

First classes. The syntax is pretty easy

<script src='https://gist.github.com/stimms/5087921.js'></script>

This short snipped of code shows off a lot of different features of the classes. First we see a class level property in the description. Next there is a constructor fo the class. You'll notice that the propertypublicationDate is prefaced by the keyword public. This is a great little shortcut which turnspublicationDate into a public property on the class. It is the same as if we had created a property like description then assigned a value to it in the constructor. Finally we see the use of the this keyword to denote that we are accessing a class level variable.

Now if we wanted to move this class into a namespace we would use the module directive

<script src='https://gist.github.com/stimms/5088269.js'></script>

Modules can be described in any number of files and the JavaScript engine will combine them together for you.

The beauty part of TypeScript and other languages which compile to JavaScript is that if you decide to abandon the technology and switch back to JavaScript then the exit cost iseffectivelyzero. TypeScript produces this, very readable, JavaScript from our module and class

<script src='https://gist.github.com/stimms/5088265.js'></script>



