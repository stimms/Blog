---
layout: post
title: Stop With the Error Codes, Already
authorId: simon_timms
date: 2013-02-12
---

I don't like error codes. I suppose that back in the day when computers were young and had very limited memory there was a purpose to error codes. But last I checked everybody and their mother had enough storage available to show people a message in their language of choosing. I'm put in mind of this today by there being some random alpha numerical error on [Windows Phone](http://www.engadget.com/2013/02/09/windows-phone-users-unable-to-download-apps-receiving-error-cod/) [](http://www.engadget.com/2013/02/09/windows-phone-users-unable-to-download-apps-receiving-error-cod/). I didn't see it myself and, honestly, I'm so done with Windows Phone that I wouldn't care if I did.

Why are we showing errors like this to users? We try to abstract the inner functioning of our applications from users so they don't know what technologies or tools were made to created it. Users don't care that you used a really novel way of building a dependency injection container or that your development workflow was centered around branches and not feature flags. Nope, they care if things work and if they are easy to use. So look deep into your developer brains and think which of these is better?


## 805a0193

or


## An error hasoccurred: the server is currently unresponsive â€“ we're working on a fix and will be back shortly

Gosh, I sure like that second one. Even more than that error I would like to see something which will tell me how I can fix the problem, if it is indeed something I can fix. As an example I have an application which reads excel files and loads them into a database. I would say that 30% of the time when I run it there is a failure because I've still got the excel file open. Instead of crashing or showing anunintelligibleerror message I show


## Unable to read Excel file. Typically this is because the file is already open. Close Excel and click to retry.

This error is precise, it gives a tip on how to fix it and a shortcut to the action the user should perform after they have solved the problem. We should be writing more errors like this. It only takes an extra 5 minutes to code up but can save people a lot in the long run.



