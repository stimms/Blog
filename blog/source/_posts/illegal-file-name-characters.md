---
layout: post
title: Illegal file name characters
authorId: simon_timms
date: 2014-01-28
---

If you're code is interacting with the file system at all in Windows then you might want to think about what sort of protection you have in place for illegal characters in file names. Previously my code was checking only a handful, a handful I got from looking at the error message windows gives you when you attempt to rename a file using one of these characters.

[![Image](http://stimms.files.wordpress.com/2014/01/illegal-file-names.jpg?w=433)](http://stimms.files.wordpress.com/2014/01/illegal-file-names.jpg)

This is but the tip of the iceberg. Instead use something like this

<script src='https://gist.github.com/stimms/8675631.js'></script>

*Efficiency of this code not tested and probably terrible



