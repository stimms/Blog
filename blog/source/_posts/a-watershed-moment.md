---
layout: post
title: A watershed moment
authorId: simon_timms
date: 2009-08-07
---

News in the twitterverse today is all about [this article](http://infoworld.com/t/software-licensing/watch-out-developers-here-come-lawyers-436) at inforworld or, more precisely, the guidelines produced by the [American Law Institute](http://en.wikipedia.org/wiki/American_Law_Institute). In a small section they suggest that software companies should be held liable for shipping software with known bugs. Damn right they should.

The software world has long survived protected from their own mistakes through the EULA. Let's look at a license agreement, perhaps the Windows XP license agreement as a typical example. Here is a excerpt from section 2.15:

*LIMITATION ON REMEDIES; NO CONSEQUENTIAL OR OTHER DAMAGES. Your exclusive remedy for any breach of this Limited Warranty is as set forth below. Except for any refund elected by Microsoft, YOU ARE NOT ENTITLED TO ANY DAMAGES, INCLUDING BUT NOT LIMITED TO CONSEQUENTIAL DAMAGES*

So if Windows XP crashes and you loose an assignment or if your [battleship](http://en.wikipedia.org/wiki/USS_Yorktown_%28CG-48%29#Smart_ship_testbed) sinks Microsoft is sorry but they aren't going to stand up and take responsibility. At least not more than the purchase price of Windows and I'll bet you it is a trial to get them to cough up even that. A lot of people are upset about this guideline. By a lot of people I, of course, mean software vendors. I suppose I too would be upset if I shipped known bad software, but I don't because my customers deserve more than that. I'm not saying my software is perfect, it isn't, however it has no known bugs. And that's the key right there: know bugs.

All software is going to have bugs in it, even with the most rigorous testing and test driven development there are still going to be issues. Most of these bugs are not covered under the guidelines because a concerted effort was made to find bugs and fix them during the development process. This doesn't mean that you can't ship software with know issues, it just means that now the risk of doing so is spread more evenly between you and your client. Which is only right.

Drawing again on the tired car analogy people would be outraged if Ford shipped a car which they knew imploded in the rain. Sure the car industry isn't a perfect analogy but it is pretty good. Lots of software is responsible for our lives in the same way as cars, heck there is a huge amount of software in cars. Why should software vendors be any less liable?

What does this really mean for companies? In a word: transparency. Let's say that I ship some software with an issue and it hurts somebody and they decide to sue. It is going to cost me money in lawyers even if I had no idea that defect existed. I've got to show that I didn't know the defect existed at shipping time. That is going to require a bunch of e-discovery and searching of e-mails, costly. What can I do to try to avoid being sued in the first place? Easy, publish all the bugs in my software in a system the public can see. I mean internal bugs as well as external bugs, everything. Next I fix bugs before I write new code and I fix them promptly. If I can gain a reputation as open and responsive people are far more likely to write off my mistakes as just that.

Now somebody is going to argue that publishing every defect puts me at a PR disadvantage compared to the guys down the street who don't advertise their bugs. I don't believe that for a second. People who buy software are generally not dumb, they know that bugs exist and they know that they're probably going to find some in the software they buy. Do you want to deal with a company which [won't admit that they have bugs](http://autonomy.com) and even threatens to [sue people who bring them issues](http://www.channelregister.co.uk/2007/12/06/autonomy_secunia_dust_up/) or with one which has shown itself to be responsive to customer issues? I'll take responsive every time.

The time has passed when software was used only for esoteric purposes by men in white coats and it is time the industry grew up and learned that if you get a paper route you can't dump the papers in the garbage without consequences. Stop dumping bad code on the world.



