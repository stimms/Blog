---
layout: post
title: Visual Studio 2015 Not Launching
authorId: simon_timms
date: 2015-02-12
---
If you're having a problem with Visual Studio 2015 not launching then, perhaps, you're in the same boat as me. I just installed VS 2015 CTP and when I went to launch it the splash screen would blink up then disapear at once. Staring with SafeMode didn't help and there was nothing in any log I could find to explain it. In the end I found the solution was to open up regedit and delete the 14.0 directories under HKEY\_CURRENT\_USER\Software\Microsoft\VisualStudio.  Any settings you had will disapear but it isn't like you could get into Visual Studio to use those settings anyway. 

![Regedit](http://i.imgur.com/ylruhON.png)
Hopefully this helps somebody.
