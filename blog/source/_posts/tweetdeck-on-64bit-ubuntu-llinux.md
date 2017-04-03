---
layout: post
title: Tweetdeck on 64bit Ubuntu Llinux
authorId: simon_timms
date: 2009-02-26
---

[Tweetdeck](http://www.tweetdeck.com/) is a very powerful twitter client which is written in the rather nifty [Adobe Air](http://en.wikipedia.org/wiki/Adobe_Air) framework. Unfortunately the air platform does not yet have support for 64-bit Linux. I imagine it will come along once flash 10 for 64 bit Linux comes out of beta. There is a rather long tutorial on how to work around the limitations and run it in 32-bit mode on 64-bit linux right [here](http://kb.adobe.com/selfservice/viewContent.do?externalId=kb408084) but that isn't enough to run tweetdeck. You'll also need to pull down a copy of 32 bit libgnome-keyring to allow it to access your keyring.

<div style="padding-left:20px;margin-left:20px;background-color:rgb(243,243,243);font-family:courier, serif;">wget http://ubuntu.interlegis.gov.br/ubuntu/pool/main/g/gnome-keyring/libgnome-keyring0_2.22.2-0ubuntu1_i386.deb  
ar x libgnome-keyring0_2.22.2-0ubuntu1_i386.deb data.tar.gz  
tar -xzvf data.tar.gz  
cp usr/lib/* /usr/lib32/</div>On a related note it is a shame that the default Air page is so pedestrian. I am filled with no excitement upon visiting it.



