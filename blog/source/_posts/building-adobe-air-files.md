---
layout: post
title: Building Adobe Air Files
authorId: simon_timms
date: 2009-04-29
---

Assembling Adobe Air installers is pretty easy. In this article I'll show you how to put together an ant build.xml file which will build for you.

For the incurably impatient here is the whole thing:

<script src='https://gist.github.com/4430857.js'></script>

Let's break it down a bit

<script src='https://gist.github.com/4430862.js'></script>

Here we set up some properties to be used in the rest of the file. I'm sure I don't have to quote [the Pragmatic Programmer](http://www.pragprog.com/the-pragmatic-programmer/extracts/tips) about repeating yourself.

Air applications have to be signed. As part of a release process you'll want to generate a proper key and keep in somewhere safe like on a cd in the cat's litter box. However we don't want developer builds getting out into the wild so we'll just generate a key here and use it.

<script src='https://gist.github.com/4430867.js'></script>

 You'll notice on line 12 we sleep for a couple of seconds. Why is that? It turns out that the key generator returns before it is actually done generating so we'll give it a few more seconds otherwise you'll get an error like

  
failed while unpackaging: [ErrorEvent type="error" bubbles=false cancelable=false eventPhase=2 text="invalid package signature" errorID=5022]  
starting cleanup of temporary files  
application installer exiting

in the Air installation log. (See [http://kb.adobe.com/selfservice/viewContent.do?externalId=kb403123](http://kb.adobe.com/selfservice/viewContent.do?externalId=kb403123) to read how to enable logging of Air installations)

Finally we come to actually building the application

<script src='https://gist.github.com/4430883.js'></script>

Here I'm replacing the version number in the application.xml with the SVN_REVISION. Our builds run from inside the wonderful [Hudson build management system](http://hudson.dev.java.net/) which is kind enough to pass through a token containing the revision number from SVN. We finally execute adt the Air packaging tool giving it a list of the files we would like to have inside the package. We list them by hand as a double check against accidentally including extra files, like bankingInformation.xls.

That's it! The only real trick is pausing for the key generation to complete.



