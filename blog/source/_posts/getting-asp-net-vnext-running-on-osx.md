---
layout: post
title: Getting ASP.net vNext Running on OSX
authorId: simon_timms
date: 2014-07-21
---

I'm giving a talk this week at the local .net user group about ASP.net vNext. I thought I would try to get it running on my Mac because that is a pretty nifty demo. 1. If you don't have mono installed at all then go grab the latest binaries. You need to have a functioning mono installation to build mono. Weird, huh? 2. Install mono from git. I set my prefix in the autogen step to be in the same directory as my current version of mono

git clone https://github.com/mono/mono.git cd mono ./autogen.sh --prefix=/Library/Frameworks/Mono.framework/Versions/3.6.1 make sudo make install

Now when I first did this I had all sorts of weird compilation problems. I messed around with it for a while but without much success. Google was no help so in a last ditch effort I pulled the latest and everything started to work again. So I guess the moral is that the cutting edge sometimes fails to build. On the other hand it would be good if the mono team had a CI server which would spot this stuff before it hit dumb end users like me.

**Update:** Mono 3.6 has been released which should fix most of the issues people were having with vNext on OSX. You don't need to build from source anymore. The updated packages should be in brew in the next little while.

3. Install homebrew if you don't already have it. You can find instructions athttp://brew.sh/

4. Use brew to install k. Whyk? I don't know but it is a prefix which is used all over the vNext stuff.

brew tap aspnet/k brew install kvm source kvm.sh

This will set up kvm which is the version manager.

5. Use kvm to install a runtime. The wiki for vNext suggests this runtime but it is really old.

kvm install 0.1-alpha-build-0446

I've been using the default latest and it seems to be more or less okay.

6. Pull down the home depot fromhttps://github.com/aspnet/Home/. This repois the meeting point for the various aspnet projects and the wiki there is quite helpful.

7. Jump into the ConsoleApp directory and run

k run

This will compile the code and execute it. It will be compiled with Roslyn which is cool enough to make me happy. There is very little printed by default but you can change that by setting an environmental variable

export KRE_TRACE=1

I did run into an issue running the sample web application and sample MVC applications from the home repo.

System.TypeInitializationException: An exception was thrown by the type initializer for HttpApi ---> System.DllNotFoundException: httpapi.dll

I chatted with some folks in the Jabbr chatroom for aspnet vNext and it turns out that the current self hosted ASP.net doesn't work full yet on OSX. However there is an alternative in[Kestrel](https://github.com/aspnet/KestrelHttpServer)a libuv based http server. I pulled that repo and tried the sample project which worked great.

If you're around Calgary on Thursday then why not come to my talk and watch me stumble around trying to explain all of this stuff?[http://www.meetup.com/Calgary-net-User-Group/](http://www.meetup.com/Calgary-net-User-Group/)



