---
layout: post
title: Typescript - Compiling your first program
authorId: simon_timms
date: 2013-03-04
---

I have been using typescript which is the newishMicrosoft language which compiles to JavaScript in a lot of my blog posts lately. It occurred to me that I haven't actually written anything about typescript by itself. I'm really a big fan of many of the things typescript does so let's dive right in.

The first thing to know is that typescript needs to be compiled by the typescript transcompiler. There are a number of ways to get the compiler. You can install it as part of a node.js install using npm, the node package manager. You can download the visual studio plugin or you can compile it from source. Compile it from source? What is this, linux? Actually the compiler is written in typescript. I guess that this is a pretty common thing to do when you build a compiler: build a minimal compiler and then use that to build a more fully featured compiler. It boggles my mind, to be perfectly honest.

Anyway, I installed the npm version because I'm doing most of my web development using the Sublime text editor. The command line is very easy, you can pretty much get away with knowing

./tsc blah.ts

This will produce a javascript file called blah.js. I recommend that you keep the default of naming the javascript files after the type script files, it will help you keep your sanity. If you really want to rename the output you can do it with the out flag

./tsc blah.ts -out thisisabadidea.js

There are a number of other flags you can use including a bunch of undocumented flags you'll only find by reading the source. The most useful flag is -w which watches the .ts file and recompiles it when there are changes. This is useful if you're developing you code and don't want to have to keep dropping to the command line to recompile and then refresh in the browser.

The first thing about the typescript language itself you need to know is thatit has a hybrid type system. Typically javascript performs type checking only at runtime, by default this is the same thing that typescript does but typescript allows you to perform some type checking by annotating your variables with a type. To do this the syntax is to add : type to a variable declaration. These can be used in functions and in declarations. For instance:

<script src='https://gist.github.com/stimms/5087563.js'></script>

If we change the call to this function to

<script src='https://gist.github.com/stimms/5087568.js'></script>

Then try to compile it typescript will throw an error

Supplied parameters do not match any signature of call target

This is obviously a very simple example but I have found that typescript quickly pulls out silly typing errors which normally I would have to check with unit tests.

Tomorrow we'll look into some more features of typescript.



