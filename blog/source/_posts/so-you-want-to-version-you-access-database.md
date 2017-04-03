---
layout: post
title: So You Want to Version Your Access Database
authorId: simon_timms
date: 2013-04-12
---

First off let me say that I'm sorry that you're in that position. It happens to the best of us. There is still a terrifying amount of business logic written in MS Access and MS Excel. One of the things I've found working with Access to be greatly improved if you use source control. This is because access has a couple of serious flaws which can bealleviatedby using source control.

The first is that access ismonolithic, it is a single file which contains forms, queries, logic and, sometimes, data. This makes shipping the database easy and doesn't confuse users with a bunch of dlls and stuff. It also means that exactly one person can work on designing the database at any time. Hello, scalability nightmare.

Next up is that access has a tendency to change things you didn't change. As soon as you open a form in design mode Access makes some sort of a change. Who knows why but it worries me. If I'm not changing anything then why is Access changing something?

Finally Access files grow totally out of control. Every time you open the database its size increases seemingly at random. This is probably an extension of the previous point.

Access is a nightmare to work with, an absolute nightmare. I have no secret inside knowledge about what Microsoft is doing with Access and Office in general but I suspect that desktop versions of office have a limited future. There have been no real updates to the programming model inâ€¦ well ever as far as I can tell.

Okay well let's put the project under source control and then I'll talk a bit about how this improves our life. I'll be using TFS for the source control because we might as well give ourselves a challenge and have twonightmares to deal with.

The first thing you'll need is the [access MSSCCI extensions](http://visualstudiogallery.msdn.microsoft.com/b5b5053e-af34-4fa3-9098-aaa3f3f007cd)followed up by the [MS Access Developer Tools](http://www.microsoft.com/en-us/download/details.aspx?id=6840). Now when you open up access there should be a new tab available to you in the menu strip: Source Control. Yay!

<div class="wp-caption aligncenter" id="attachment_2630" style="width: 760px">[![Menu bar additions](http://stimms.files.wordpress.com/2013/04/menubar.jpg)](http://stimms.files.wordpress.com/2013/04/menubar.jpg)Menu bar additions

</div>Open up your current database and click the button marked Add Database to Team Foundation. You'll be prompted for your TFS information. Once that's been entered access will spool up and create a zillion files in source control for you. This confused us a lot when we first did it because none of the files created were mdb or accdb files: the actual database. Turns out the way it works is that the files in source control are mapped, one to one, with objects in the database. To create a â€œbuildâ€ of the database you have to click on the â€œCreate from Team Foundationâ€ button. This pulls down all the files and recombines them into the database you love.

<div class="wp-caption aligncenter" id="attachment_2631" style="width: 473px">[![Selecting the TFS source (identifying information removed)](http://stimms.files.wordpress.com/2013/04/select-source.jpg)](http://stimms.files.wordpress.com/2013/04/select-source.jpg)Selecting the TFS source (identifying information removed)

</div>You'll now see that the object browser window now has hints on it telling you what's checked out.Unfortunately you need to go and check out objects explicitly when you work on them. At first it is a pain but it becomes just part of your process in short order. One really important caveat is that you have to do the source control operations through the access integrations, you can't just use TFS from Visual Studio. This is because the individual source files are not updated until you instruct access to check them in. Before that changes remain part of the mdb file and are not reflected in the individual files.

Right so what does this do for us? First having the code and objects split over many files improves the ability to work on a databasecollaboratively. While the individual objects are a total disaster ofserializationindividualscan still work on different parts of the same database at once. That's a huge win. Second we're protected from Access' weird changes to unrelated files. If we didn't change something then we just revert the file and shake our heads at Access. Finally because the mdb file is recreated each time we open it there is no longer unexpected growth.

This doesn't make working with Accesspainlessbut it sure helps.



