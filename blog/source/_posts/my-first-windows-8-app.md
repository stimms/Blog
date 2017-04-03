---
layout: post
title: My First Windows 8 App
authorId: simon_timms
date: 2013-05-28
---

I have been putting off building a Windows 8 app for ages. I actually don't have a a computer in the house which runs Windows 8 on bare metal but I do have a couple of VMs. Well the impending end of the [developer movement](http://developermovement.ca)finally got me going(it ends on the 15th of June, BTW). I had read something a few months back about an open source Windows 8 app which republished your WordPress blog as an app. Perfect!

<div class="wp-caption aligncenter" id="attachment_2757" style="width: 760px">[![Source: http://technogerms.com/windows-8-review/](http://stimms.files.wordpress.com/2013/05/windows-8-wallpaper.jpg?w=750)](http://stimms.files.wordpress.com/2013/05/windows-8-wallpaper.jpg)Windows 8: the wettest windows not found on a submarine.

</div>[Ideapress](http://ideapress.me/)is the project and they have a pretty simple HTML/JavaScript app which pulls down your posts and republishes them. On their site is even a wizard for creating the app. I ran through the wizard and gave it various images and an API key for wordpress.com. At the end of the process(which finished neither late, nor early butpreciselywhen it meant to) I had a zip file with a VS2012 project in it.

I threw it on a Windows 8 box (as you can only develop for Windows 8 on Windows 8) and hit F5 to run it. I ended up with a pretty darn good app. I was just about to call it good enough and publish it when I realized that none of the source code on my blog was showing up.

I use embedded gists to show source code an provide syntax highlighting. WordPress has built in support for gists. It isn't actually difficult to do as if you append .js to the end of a gist you can get a javascript version of the gist which injects styling and a div onto your page. Pretty smart. This wasn't working on the app for some reason, likely security related. The security model for injecting content into the page in Windows 8 is more restrictive than on a browser.

In order to fix that I pulled in a handy library called [gist-embedded](https://github.com/blairvanderhoof/gist-embed). This library made us of jQuery which is where I ran into my first problem. I guess that before jQuery 2.0 there were somecompatibilityissues between jQuery and the Windows 8 runtime. These have been resolved in jQuery 2.0. However, my application failed validation due to the jQuery file which nuget added. I was confused because I failed in two sections. The first was that the code didn't run and the second was that the encoding was wrong. The encoding was easy enough to fix. You just resave the jquery file with UTF-8 encoding.

<div class="wp-caption aligncenter" id="attachment_2761" style="width: 760px">[![Changing the encoding](http://stimms.files.wordpress.com/2013/05/screen-shot-2013-05-23-at-10-42-08-am.jpg?w=750)](http://stimms.files.wordpress.com/2013/05/screen-shot-2013-05-23-at-10-42-08-am.jpg)Changing the encoding

</div>But I was totally flumoxed by thecompilationerrors. I could not manage to duplicate them in the app or in IE10. Eventually I ran the validation again and found everything now worked. I guess the incorrect encoding was causing more than one issue.

Now that I had the application working the way I wanted and passing the validation I signed up for the windows store. I had to give Microsoft $49 of my hard earned dollars for the privilege of giving away a freeapplication. It is so funny that companies likeMicrosoftand Apple have tricked us into paying for the right to release software on their platform. If he wasn't a mildly insane toe nail eater I would beinterestedto hear what [RMS](http://en.wikipedia.org/wiki/Richard_Stallman) would say about it.

Weirdly Microsoft validate your credit card by charging a small amount then refund it and use it as a method of validating your account. They require that you enter the amount charged to validate your account. This means that if, like me, your bank sucks at posting charges with any rapidity you'll be stuck waiting.

When setting up the application you have to enter a stunning amount of meta-data about where you want your app advertised, which age groups, what category, whether it contains cryptography"¦ it wasoverwhelming and at least half the stuff could have been replaced by setting some reasonable defaults.

At last I got to the point where I could submit my app for publishing.Yay! Now I can sit here and wait. And wait.

<div class="wp-caption aligncenter" id="attachment_2762" style="width: 617px">[![Steps](http://stimms.files.wordpress.com/2013/05/screen-shot-2013-05-23-at-10-55-28-am.jpg?w=607)](http://stimms.files.wordpress.com/2013/05/screen-shot-2013-05-23-at-10-55-28-am.jpg)Steps

</div>The phase which was suppose to take an hour took a day. I'm now into hour 20 of the 3 hour security testing. I'm obviously more excited about getting this app out than I should be. No amount of refreshing seems to speed the process along. It is superunfortunatethat this process is so time consuming.

I can't comment on the rest of the process because I'm still waiting.

Overall I think that building and distributingWindows 8 apps is more painful than it needs to be. I suppose you do get access to a global distribution channel and a place in the Windows store for your $49 but if uptake on Metro apps has been as bad [as reported](http://www.businessinsider.com/windows-8-users-dont-use-metro-apps-2013-5)then cleaning up app submission should be high on a priority list at Microsoft.

The submission process needs to get faster. I know that the process is far better than Apple's but it still sucks. I'm use to hitting publish in my CI system and 5 minutes later everything is up and running on my site. I don't expect updates to be pushed out to clients with that rapidity but I do expect the app to at least be published in less than a day. App stores are the antithesis<span style="font-size:13px;line-height:19px;">of the continuous deployment model.</span>

Actually programming an app using JavaScript is great. I really like the JavaScript and HTML model so, so much more than dealing with XAML and C#. I'll write more about then when I talk about writing my first Windows Phone 8 app. The security model take some getting use to and I haven't tried a framework like AngularJS out yet so I can't comment much on that.

Is it worth making Windows 8 specific apps? That's a firm maybe. I don't think it should be the default for any company but if there is a customer base asking for it then you might as well. It is no more difficult than building a normal application.





