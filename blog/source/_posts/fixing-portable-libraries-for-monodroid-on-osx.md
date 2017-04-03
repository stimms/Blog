---
layout: post
title: Fixing Portable Libraries for MonoDroid on OSX
authorId: simon_timms
date: 2013-01-01
---

Over the Christmas break I thought I would play a bit with MonoDroid or Mono for Android as I guess it is now called. Things were going pretty well until I decided to split the data access components into their own library. I was on v3.05 and compiling resulted in an encounter with thediabolical issue [7905](https://bugzilla.xamarin.com/show_bug.cgi?id=7905 "Diabolical Issue"). The fix suggested in that was altering

> /Library/Frameworks/Mono.framework/Versions/2.10.9/lib/mono/xbuild/Microsoft/Portable/v4.0/Microsoft.Portable.CSharp.targets

To change

<script src='https://gist.github.com/4429184.js'></script>

to

<script src='https://gist.github.com/4429197.js'></script>

This did get me most of the way there but I ran into the error

> targetframeworkversion v1.0 could not be converted to an android api level

Digging around a little bit with that I found that in the same file altering thepropertiesdefined at the top to

<script src='https://gist.github.com/4429225.js'></script>

Corrected the issue. It was a bitdisappointingthat this didn't work out of the box. I would have thought that creating portable libraries would be a pretty common use case for Mono for X platforms.



