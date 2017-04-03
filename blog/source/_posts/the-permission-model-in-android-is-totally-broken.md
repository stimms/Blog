---
layout: post
title: The permission model in android is totally broken
authorId: simon_timms
date: 2014-04-22
---

When installing a new application on a cell phone I typically agree to whatever the stupid app wants. My approach is â€œjust do it and stop asking me questionsâ€. There have been [numerous](http://www.techinasia.com/35-android-apps-secretly-stealing-private-data-chinas-latest-dcci-report/) [reports](http://www.bbc.co.uk/news/technology-25258621) about how apps are stealing data. I had to rebuild my phone this week after getting a replacement from Google due to some rather nasty screen issues. I thought I would be a bit more circumspect in installing applications this time. I took a close look at the permissions applications were requesting as I installed them.

It is absolutely amazing the permissions applications are requesting. Of the 10 or 11 clock applications I looked at every last one of them wanted some permission which I deemed unnecessary. Reading caller ids, access to the network, access to contacts, ability to send e-mails without me knowing,â€¦ Outrageous! I'm sure an argument could be made for many of these but I cannot imagine how the argument for being able to read my text messagesor read my contacts would go. If you're not paying for something then you're the product has never been more true.

<div class="wp-caption aligncenter" id="attachment_3304" style="width: 760px">[![Asking to read my text messages? That's a paddlin'](http://stimms.files.wordpress.com/2014/04/screenshot_2014-04-22-06-38-05.png)](http://stimms.files.wordpress.com/2014/04/screenshot_2014-04-22-06-38-05.png)Asking to read my text messages? That's a paddlin'

</div>What's the solution?

I think it is actually a pretty easy solution: grant permissions in the same way as HTML5 or OpenID. HTML5 will request permission when a page performs some activity such as capturing images from your web camera. If the script isn't granted permission to access the camera then it should degrade or cancel based on this. Equally when you're logging into an OpenID site and it requests additional fields from the login provider then you can click cancel and the application should accept this and compensate.

<div class="wp-caption aligncenter" id="attachment_3302" style="width: 760px">[![Sorry, you need what permissions?](http://stimms.files.wordpress.com/2014/04/screenshot_2014-04-22-06-21-10.png)](http://stimms.files.wordpress.com/2014/04/screenshot_2014-04-22-06-21-10.png)Sorry, you need what permissions?

</div>As it stands I either accept that my alarm clock needs to read my text messagesor I don't install it. Usually I just don't install it. If I were able to pick and chose the permissions the application could have then it could degrade and still give me some functionality. Developers would have a much harder time sneaking malware onto phones if this could be done. As an added bonus I would like to see developers have to enter a reason why each permission was needed and have that show up during the install.[  
](http://stimms.files.wordpress.com/2014/04/screenshot_2014-04-22-06-21-10.png) [  
](http://stimms.files.wordpress.com/2014/04/screenshot_2014-04-22-06-34-24.png) [  
](http://stimms.files.wordpress.com/2014/04/screenshot_2014-04-22-06-38-05.png)

<div class="wp-caption aligncenter" id="attachment_3303" style="width: 760px">[![The correct set of permissions for an alarm clock](http://stimms.files.wordpress.com/2014/04/screenshot_2014-04-22-06-34-24.png)](http://stimms.files.wordpress.com/2014/04/screenshot_2014-04-22-06-34-24.png)The correct set of permissions for an alarm clock

</div>I can't believe that Google is just letting this stuff go. Say what you will for Apple but they're pretty willing to crack down on stuff like this.

[](http://stimms.files.wordpress.com/2014/04/screenshot_2014-04-22-06-38-05.png)



