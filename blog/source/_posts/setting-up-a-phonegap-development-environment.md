---
layout: post
title: Setting up a PhoneGap Development Environment
authorId: simon_timms
date: 2013-04-16
---

There's probably already a zillion posts out there about how to set up a PhoneGap development environment on OSX. I wanted my own because I did it once and now I've forgotten how. Typical idiot thing to do.

First download the Android Development Tools from [here](http://developer.android.com/sdk/index.html#download). There are some [installation instructions](http://developer.android.com/sdk/installing/bundle.html) but they basically amount to "run unzip". This bundle include eclipse and a swack of Androidy stuff. I moved the contents to a Tools directory, because that's how I roll.

In to that directory I also put the latest version of phone gap. At the time of writing that was 2.6.0.

I added all the tools directories to my PATH in .bash_profile

echoexport PATH=${PATH}:~/Tools/sdk/tools:~/Tools/sdk/platform-tools:~/Tools/phonegap/lib/android/bin

Here if there was a need to do iOS I would replace android in the last part of the path with ios. Or, perhaps it is opposite day and BlackBerry is a popular platform with a strong future then do s/android/blackberry/. But, let's be honest, if it's opposite day then I'm a Russian oil billionaire and I'm probably out hunting with Putin.

<div class="wp-caption aligncenter" id="attachment_2598" style="width: 630px">[![That's me, on the right.](http://stimms.files.wordpress.com/2013/04/putin.jpg)](http://stimms.files.wordpress.com/2013/04/putin.jpg)That's me, on Putin's right.

</div>It is kind of a pain that the scripts to create PhoneGap projects are all named the same but just in different directories. If I were making a lot of new apps then probably I would just alias a ton of them instead of adding to my path.

alias create-ios=~/Tools/phonegap/lib/ios/bin/create alias create-android=~/Tools/phonegap/lib/android/bin/create alias create-hahahaha=~/Tools/phonegap/lib/blackberry/bin/create

That's pretty much it. New projects are created by running the create script and then pointing eclipse at the folder.



