---
layout: post
title: 2 Days with ScriptCS
authorId: simon_timms
date: 2013-10-10
---

A couple of days ago I found myself in need of doing some programming but without a copy of Visual Studio. There are, obviously, a million options for free programming tools and environments. I figured I would give ScriptCS a try.

If you haven't heard of ScriptCS I wouldn't be surprised. I don't get the impression that it has a huge following. It is basically a project which makes use of the Roslyn C# compiler to build your shell script like C# into binaries and execute them. It provides a REPL environment for C#. On the surface that seems like a pretty swell thing. My experience was mixed.


# The Good

Having the syntax around from C# made my life much easier. I didn't have to look up any syntaxes for loops or the whatnot which always seems to catch me when I script something in a more traditional scripting language. It was also fantastic to have access to the full CLR. I could do things quickly which would have been a huge pain to figure out in other scripting languages like access a database.

You can also directly install nuget packages using scriptcs simply by running

scriptcs.exe -install DocX

It will download the package, create or update a packages.cs and the scripts automatically find the packages without having to explicitly include the libraries. I was able to throw together a tool which manipulated word documents with relative ease.

I had a lot of fun not having access to Intellisense. At first I was concerned that I really didn't know how to programme at all and that everything I did was just leaning on a good IDE. After an hour or so I was only slightly less productive that I would have been with Visual Studio. I used sublime as my editor and threw it into C# mode which gave me syntax highlighting. Later I discovered a sublime plugin which provided C# completion! Woo, if not for coderush sublime could have replaced Visual Studio outright.

It was easy to define classes within my scripts something I find cumbersome in some other scripting languages. The ability to properly encapsulate functionality was a joy.

I didn't try it but I bet you would have no problem integrating unit tests into your scripts which puts you a huge step up on bash"¦ is there a unit testing framework for bash? (Yep there is:[https://github.com/spbnick/epoxy](https://github.com/spbnick/epoxy)).


# The Not So Good

Of course nothing is perfect. I used scriptCS as one would use bash. I would write a chunk of code then run the script, check the output and then add more code. Problem is that scriptcs is SLOW to start up. Like kicking off a fully fledged compiler slow. This wouldn't have been too bad except that for some reason every time I ran a script it would lock the output file.

C:tempscriptcs> scriptcs.exe .test1.csx ERROR: The process cannot access the file 'C:tempscriptcsbintest1.dll' because it is being used by another process. C:tempscriptcs> del bin* C:tempscriptcs> scriptcs.exe .test1.csx hi C:tempscriptcs>

I wanted to stab the stupid thing in the face after 5 minutes. I opened up process explorer to see if I could see what was locking the file. As it turns out: nothing was using the file. I don't know if this is a bug in windows or in scriptcs. In either case it is annoying. I discovered that you can pass the -inMemory flag which avoids the file locking issue by not writing out a file. I guess this will become the default in the future but it brings me to:

The documentation isn't so hot either. I get that, it is a new project and nobody wants to be slowed down by having to write documentation. However I couldn't even find documentation on what the flags are for scriptcs. When I went to find out how to use command line arguments I could only find an inconclusive series of discussions on a bug.


# The Bad

There were a couple of things which were so serious they would stop me from running ScriptCS for anything important. The first was script polution. If you have two scripts in a folder when running the second one you'll get the output from the first script. Yikes! So let's say you have

delete-everything.csx send-reports-to-boss.csx

running

scriptcs send-reports-to-boss.csx

will run delete-everything.csx. Ooops. (Already documented as[https://github.com/scriptcs/scriptcs/issues/475](https://github.com/scriptcs/scriptcs/issues/475))

I also ran into a show stopping issue with using generic collections which I further documented here:[https://github.com/scriptcs/scriptcs/issues/483](https://github.com/scriptcs/scriptcs/issues/483)

The final show stopper for me making more use of ScriptCS is that command line argument passing hasn't really been figured out yet. See the issue is that passing arguments normally passes them to ScriptCS instead of the script

scriptcs.exe .test1.csx -increase-awesome=true

the solution seems to be that you have to add "” to tell scriptcs to use that argument for the script

scriptcs.exe .test1.csx -- -increase-awesome=true

However some version of powershell hate that. The issue is well documented in[https://github.com/scriptcs/scriptcs/issues/474](https://github.com/scriptcs/scriptcs/issues/474)


# Am I going to Keep Using It?

Well it doesn't look like I'll be getting a full environment any time soon in this job. As such I will likely keep up with ScriptCS. I hate not having a real environment because it means I can't contribute back very well. Although my discovery of C# completion in Sublime might change my mind"¦

If scriptcs worked on mono(it might, I don't know) and if there was a flag to generate executables from scripts I would be all over it. It is still early in the project and there is a lot of potential. I'll be keeping an eye on the project even if I don't continue to use it.



