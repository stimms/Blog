---
layout: post
title: Breaking Excel Passwords
authorId: simon_timms
date: 2014-03-12
---

If you've ever built and sold an excel add-in written in VBA you've probably wanted to hide your code so that nobody else can get a hold of it. The problem with VBA is that it is pretty easy to extract and edit. Microsoft have, over the years, made some attempts to lock down the file format with passwords and encryption and the such. They generally haven't worked very well.

Today I encountered a very solid attempt to thwart user editing of VBA. Typically you just need to follow the steps listed on [StackOverflow](https://stackoverflow.com/questions/272503/how-do-i-remove-the-password-from-a-vba-project/7835861#7835861). This time, however, these tricks didn't work. When attempting to expand the project node in the VBA editor this error was thrown:

[![Screen Shot 2014-03-11 at 10.54.23 PM](http://stimms.files.wordpress.com/2014/03/screen-shot-2014-03-11-at-10-54-23-pm.jpg)](http://stimms.files.wordpress.com/2014/03/screen-shot-2014-03-11-at-10-54-23-pm.jpg)Typically this Project Locked â€“ Project is unviewable error is shown the excel file has been placed into shared mode. Shared mode disables all editing of VBA. Setting the workbook to shared mode and then exclusive mode is usually enough to clear this flag. In this case, though, that didn't help. I suspect that the author of this particular excel file had used one of the tools for locking VBA. This tool must, in some way, set the shared flag in a way that it cannot be unset.

I went down many a blind alley trying to solve this. VBA code is not stored in a text format but rather in a binary blob which lives inside the open Office Open XML format: vbaProject.bin. This file format is outline a bit by Microsoft in a long and probably very boring [document](http://download.microsoft.com/download/2/4/8/24862317-78f0-4c4b-b355-c7b2c1d997db/%5BMS-OVBA%5D.pdf). I say â€œprobably very boringâ€ because I didn't read it. I would be very interested in looking at this file in a hex editor and seeing what the locking tool changes.

There are some paid services out there which promise to unlock your file. That all seemed pretty sketchy to me.

Fortunately the locking of this binary file is ignored by other tools. I used a great little tool called [VBADiff](http://www.vbadiff.com/)which was able to extract the majority of what was needed from the excel file. It wasn't able to extract the forms but they were pretty easily recreated.

I'm super impressed with the excel locking tool and the author's knowledge of the excel file format. However even all that work was still bypassed with few hours work. It goes to show that any code running on your machine can be exploited.



