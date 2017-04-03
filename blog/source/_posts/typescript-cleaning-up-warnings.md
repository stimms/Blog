---
layout: post
title: Typescript - Cleaning up Warnings
authorId: simon_timms
date: 2013-03-15
---

When you're getting started with typescript you're probably going to run into a bunch of warnings and errors. Pay attention to the messages, I've found them to be almostentirelycorrect. However you will see errors of the form

The name $ does not exist in the current scope

Why that's jQuery, why doesn't typescript know about jQuery? Well typescript doesn't know about anything other than what's in the current file. It is a dumb as a muffin. A tasty, tasty cranberry orange muffin. Maybe it has thatstreuseltoping on it"¦ sorry, I really like muffins. What can you do to solve this problem? Well that's where definition files come into play. Definition files are, in effect, C include files. They define the public interfaces of the libraries you're trying to use. Libraries like jQuery and d3.js. Libraries like your own libraries.

If you're referencing your own typescript file then you don't need to create a declarationfile. Typescript can read its own typescript files so there is no need to generate declarationfor them unless you're distributing the declarationto other developers to develop against. To instruct the transcompiler to make use of either an external typescript file or declarationfile you can include a reference in your typescript file.

/// <reference path="typings/jquery/jquery.d.ts" /> /// <reference path="PageScripts/Shared/Menu.ts" />

*  
*You can see here that I pulled in both a declarationfile for jQuery and a typescript file I created.

If you're looking for a declarationfile for apubliclydistributed library then there are some great options. You can manually download it from Boris Yankov's github repo at[https://github.com/borisyankov/DefinitelyTyped](https://github.com/borisyankov/DefinitelyTyped). Or you can grab a package off [nuget](http://nuget.org/packages?q=Definitelytyped). If you're using node then you can install the typescirpt definition tool from[http://www.tsdpm.com/](http://www.tsdpm.com/)and install packages using.

`tsd install node`

To generate your own definition files from a typescript file you can pass "“declaration to the transcompiler. This will,unfortunately only generate from typescript files. If you want to generate declarations from pure javascript then you'll need to do it by hand.



