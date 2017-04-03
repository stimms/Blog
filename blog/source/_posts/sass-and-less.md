---
layout: post
title: SASS and Less
authorId: simon_timms
date: 2013-05-13
---

This is a brain dump of a session given by cat lover [Eden Rohatensky](https://twitter.com/edenthecat).

- <span style="line-height:13px;">CSS is limited because it is designed to be simple</span>
- Simplicity leads to limited expressibility
- CSS preprocessors allow for adding a programming paradigm over top of CSS
- Using a CSS preprocessor allows for writing more maintainable code
- SASS - SASS uses a Ruby preprocessor but you can survive without Ruby as SASS is basically just a DSL
- There are two different sytaxes for SASS: SCSS is close to the CSS syntax where the origianal SASS dialect is whitespace delimited
- Variables are denoted using a $ symbol

- Eden uses Crunch as a tool for SASS
- Supports lists which are a handy construct for creating elements which are similar
- Less - Less is written in JavaScript
- Less uses @ to denote a variable
- Eden uses Compass for LESS
- Both tools support nesting of classes so you can apply the same rules for a set of classes like a:hover and a:visited
- There are some libraries of mixins to deal with things like vendor prefixes. This allows cleaning up

-webki-border-radius: 5px; -moz-border-radius: 5px; -ms-border-radius:5px; -o-border-radius: 5px; border-radius: 5px;

- Lots of functions in both but you end up in
- luma is theperceivedlightness of a color
- The compilers for SASS and Less provide some logic checking to prevent you from writingsyntacticallyincorrect code
- The errors which come out of sass

Thoughts

SASS seems like a more powerful but slightly more complicated language. I wonder if source maps could be used to transition from CSS back to the source files. Gosh I hope there are already tools written to perform SASS/Less compilation as part of the VS build process and that I dont have to write it myself.



