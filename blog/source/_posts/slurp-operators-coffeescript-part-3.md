---
layout: post
title: Slurp -  Operators CoffeeScript Part 3
authorId: simon_timms
date: 2013-04-23
---

CoffeeScript brings a number of renamed and new operators to your JavaScript. The first one is our old friend ==. JavaScript has two equality operators == and ===. The difference is that == will not perform type conversion before performing the comparison. That has the result of allowing really strange things to be true

<script src='https://gist.github.com/stimms/5440377.js'></script>

Fortunately CoffeeScript clears this weirdness up by masking JavaScript's == and introducing its own == operator which compiles to ===.

Confused yet? It's a lot of =s that's to be sure. To maintain consistency with the conversational tone of CoffeeScript you can use English words in place of the standard operators. For instance that == can be written as is.

<script src='https://gist.github.com/stimms/5440409.js'></script>

Taken from the CoffeeScript documentation

<table><tbody><tr><th>CoffeeScript</th><th>JavaScript</th></tr><tr><td><tt>is</tt></td><td><tt>===</tt></td></tr><tr><td><tt>isnt</tt></td><td><tt>!==</tt></td></tr><tr><td><tt>not</tt></td><td><tt>!</tt></td></tr><tr><td><tt>and</tt></td><td><tt>&&</tt></td></tr><tr><td><tt>or</tt></td><td><tt>||</tt></td></tr><tr><td><tt>true, yes, on</tt></td><td><tt>true</tt></td></tr><tr><td><tt>false, no, off</tt></td><td><tt>false</tt></td></tr><tr><td><tt>@, this</tt></td><td><tt>this</tt></td></tr><tr><td><tt>of</tt></td><td><tt>in</tt></td></tr><tr><td><tt>in</tt></td><td id="aeaoofnhgocdbnbeljkmbjdmhbcokfdb-mousedown">*<small>no JS equivalent</small>*</td></tr></tbody></table>In addition to being able to use the operators as one would in JavaScript one can use nifty post statement branches.

<script src='https://gist.github.com/stimms/5440465.js'></script>

The final goodie for today is the existential operator. It is represented using the ? symbol. It can be used to check to see if a value is null or undefined.

<script src='https://gist.github.com/stimms/5440623.js'></script>

A more impressive use is in operator chaining

<script src='https://gist.github.com/stimms/5440631.js'></script>

You can't tell me that isn't a far cleaner and easier to understand syntax.



