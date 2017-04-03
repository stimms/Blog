---
layout: post
title: Background Tasks in ASP.net Redux
authorId: simon_timms
date: 2014-05-23
---

A short while ago I wrote about how bad it is running [background tasks in ASP.net](http://blog.simontimms.com/2014/02/20/background-tasks-in-asp-net/ "Background Tasks In ASP.net"). It basically comes down to "you don't know when the application will recycle and your task will be killed". My solution was to farm out background tasks to another machine through the use of a messaging system the likes of Azure Service Bus or MSMQ. I still believe that this is a great solution for running in the cloud. The issue with the cloud is that not only may the app pool recycle but the whole machine might disappear and pop up on another physical machine somewhere else in the data center. There may, however be scenarios in which you might like to perform an asynchronous actions on the server itself without having to worry about app pool recycles.

Until now there has been no way to do this. With .net framework 4.5.2 that all changes.

First a warning: at the time of writing 4.5.2 is very fresh. It is not yet supported on Azure or probably most other hosts you might use. To develop against it you'll need the Microsoft .NET Framework 4.5.2 Developer Pack which can be downloaded [here](http://www.microsoft.com/en-us/download/details.aspx?id=42637). 4.5.2 will eventually be installed on Azure and should also be included in updates to Windows and Visual Studio.  I've never been able to get .net framework version adoption numbers out of Microsoft so I have no idea how long it will be before you can reasonably expect 4.5.2 to show up on the majority of machines. It really doesn't matter for this feature as you only need updates to your servers.

The first thing you'll need is to acquire the new developer pack and install it. Next you'll need to update the target for your current application to target .net 4.5.2. [![framework](http://stimms.files.wordpress.com/2014/05/framework.png)](http://stimms.files.wordpress.com/2014/05/framework.png) Getting to be quite a few platforms in there now, huh? Let's not look at the portable class library options.

A real good example background task is sending e-mail. This is an operation which can take quite a long time(relatively speaking) and is not, typically, something which needs status information fed back to the user. This, as it turns out, is a bit more complicated than we would like. There is a huge [post](https://stackoverflow.com/questions/8747483/proper-way-to-asynchronously-send-an-email-in-asp-net-am-i-doing-it-right) over on StackOverflow about how to correctly send an asynchronous e-mail. Much of the confusion comes in around sending mail asynchronously. If you look at the SmtpClient class there are now two different flavours of sending async: SendAsync and SendMailAsync. The first is the old method for sending asynchronously complete with a cancellation token nobody has ever used. The second is the newer method which uses the async/await format. The old method caused a lot of issues with there was a problem sending mail. The errors would frequently be swallowed or bubble up so high as to blow up the worker role. This was because it executed outside of the normal page context so people forgot that their normal error catching code wouldn't intercept issues. Whenever .net 5 is release I hope Microsoft make removing the old method a breaking change. To do so they really should have marked the method as deprecated in this release - perhaps for .net 6 then.

In this example we'll actually use the newer SendMailAsync method and we'll do it in a background thread. Using async allows fewer threads to be used in situations where there is some load on the system we should get better performance.

<script src='https://gist.github.com/stimms/5c10e68bd89b09e0222c.js'></script>

You will almost certainly wish to add some additional error checking in there. You should hook into UnobservedTaskException as the method returns a void task.


## A Final Warning

A background task started in this fashion still only delays the recycling of an app pool for 30 seconds. Thus if your task takes longer than 30 seconds to execute it will still be killed. For long running tasks you are still far better saving them to a persistent queue or some other storage.


## References:

http://www.davidwhitney.co.uk/Blog/2014/05/09/exploring-the-queuebackgroundworkitem-in-asp-net-framework-4-5-2/

http://blogs.msdn.com/b/dotnet/archive/2014/05/05/announcing-the-net-framework-4-5-2-release.aspx



