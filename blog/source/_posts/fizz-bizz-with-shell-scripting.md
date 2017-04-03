---
layout: post
title: Fizz Bizz with Shell Scripting
authorId: simon_timms
date: 2013-03-12
---

It seems like I'm writing a lot of fizzbizz examples at the moment. It is kind of fun experimenting with different languages. There are always different constructs for looping and recursing. I'm also super happy that there are a lot of languages to get through before I have to write it in prolog. I have prettytraumaticmemories of prolog from university. Today's language is shell script.

Of course shell script isn't just one language, it depends on which shell you're using. When I worked a lot with unix derivatives I mostly worked with bash scripting. Unless it was aparticularlyold or odd OS in which case we would end up on plain sh. There are a bunch of other shells out there and I can remember a time when both csh and ksh were also popular. I can also remember when druids sacrificed goats. Is there a link between ksh's stupidarcanesyntax and goat slaughter? I can't proveconclusivelythat there is but there are no dead goats here and no ksh syntax either. Draw your own conclusions.

I thought I would try using zsh style scripting for fizz bizz. Zsh is a newer shell which has many of the features of bash and also borrows from other shells. Now I say "newer" but it still dates to 1990.

<script src='https://gist.github.com/stimms/5122238.js'></script>

The script starts with a sha-bang which instructs the program loader that to run the script it should execute /bin/zsh which is where zsh lives on my machine. It might be better to replace it with #!/usr/bin/env zsh which instructs the program loader to launch env which searches for zsh. This allows for searching of the path for zsh. There is an increased security risk with doing so as an alternate zsh might be selected. However this risk is probably worth it for increased portability.

I was hoping that it was possible to putmathematicalexpressions in the case statements but that's not possible. Instead I took advantage of case statements here and the fact that we can take the remainder modulo 6 to do most of the fizz bizz heavy lifting. On line 3 there is a bit of a syntactic oddity. Zsh is not really designed for doing a lot of math so arithmetic operations need to live inside double parenthesis.

Gosh, I can't wait for the next interview where I'm asked about fizz bizz. I am going to kill that question.



