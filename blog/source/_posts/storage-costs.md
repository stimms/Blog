---
layout: post
title: Storage Costs
authorId: simon_timms
date: 2013-07-16
---

Earlier this week I got into a discussion at work about how much storage an application was using up. It was an amount which I considered to be trivial. 20gig, I think it was. I could have trimmed down by but it would have taken me an hour or two and with the cost of storage it didn't seem worth it. My argument was that with the cost of storage these days it would cost the company more to pay for me to reduce the file storage than to just pay for storage. The problem seemed to go away and I claimed victory.

> I'm just saying, don't start a "storage is expensive" argument with me. You're not going to win for numbers under 50TB
> 
> "” Simon Timms (@stimms) [July 8, 2013](https://twitter.com/stimms/status/354263391038214145)

<script async="" charset="utf-8" src="//platform.twitter.com/widgets.js"></script>

My victory glow did not last long. @HOVERBED, my good sysadmin friend, jumped on my argument.

"Disk is cheap, storage isn't"

Then he said some nasty stuff about developers which I won't repeat here. I may have said some things about system admins prior to that which started the spiral.

Truth is that he's right. When I talk about storage being cheap I am talking about disk being cheap. There is a lot more to storage than putting a bunch of disks in a server or hooking up to the cloud. These things are cheap but managing the disk isn't. There is a cost associated with backing up data, restoring the data and generally managing disk space. There is also the argument that server disk isn't the same as workstation disk. Server space is far more expensive because it has to be reliable and it has to be larger than typical disk. When I did the math I figured disk might cost something like $5 a gigabyte to provide. @HOVERBED quoted me numbers closer to $90 a gig. That's crazy. I'm going into go out on a limb here but if you're paying that sort of money for managing your storage you're doing it wrong. The expensive things are

1. Backing up your storage to tape andshippingthat tape to somwhere safe

2. Paying people to run the backups and restores

3. Paying for SANs or NAS which is much more expensive than just disk

So let's break this thing down. First off why are we backing up to tape still? I've seen a couple of arguments. The first is that tape is less costly than backing up to online storage. I had to look up some information on tapes because when I was last involved with them they were 250GB native. Turns out that they're up to 5TB native now(StorageTek T10000 T2). That's a lot of storage! Tapes have two listed capacity: a native capacity and a compressed capacity. The compressed capacity is 2x the native capacity "“ the theory being that you can gzip files on the tape and get extra capacity for free. I don't know if people still get 2x compression with newer file formats as many of them integrate compression already.

These tapes go for something like $150, so that's pretty frigging cheap! To get the same capacity on cloud services will cost you

<table><tbody><tr><th>Service</th><th>Per GB per month</th><th>Per 5TB per month</th></tr><tr><td>Azure Geo Redundant</td><td>$0.095</td><td>$415</td></tr><tr><td>Azure Local</td><td>$0.07</td><td>$330</td></tr><tr><td>Azure Geo Redundant</td><td>$0.10</td><td>$425</td></tr><tr><td>Amazon Glacier</td><td>$0.01</td><td>$50</td></tr></tbody></table>(Some of the math may seem wonky here but there are discounts for storing a bunch of data) Tapes are looking like a pretty good deal! Of course there are a lots of additional costs around that $150. We're not really comparing the same thing here: you need to keep multiple tapes so that you can cycle them offsite and even from day to day. I don't know what a normal tape cycling strategy is but I don't really see how you could get away with fewer than 3 tapes per 5TB.

There is also the cost of buying a tape drive and you have to pay some person to take tapes out of the drive, label them(please label them) put them in a box and ship them offsite. This takes us over to the second point: people. No matter what you do you're going to have to have people involved in running a tape drive. These people add cost and errors to the system. Any time you have people involved there is a huge risk of making a mistake. You can't tell me that you've never accidentally written over a tape which wasn't meant to be reused yet.

Doing your backup to a cloud provider can be completely automated. There are no tapes to change, no tapes to ship to Iron Mountain(which I discovered isn't actually located in a mountain). There is a bandwidth cost and the risk of failure seems to be higher when backing up offsite. Bandwidth is quickly dropping in price and as most of your bandwidth will be used overnight that means that during the day your users can benefit from way faster Internet.

<div class="wp-caption aligncenter" id="attachment_2931" style="width: 420px">[![Not what Iron Mountain looks like. Jerks.](http://stimms.files.wordpress.com/2013/07/cheyenne-mountain-operations-center1.jpg)](http://stimms.files.wordpress.com/2013/07/cheyenne-mountain-operations-center1.jpg)Not what Iron Mountain looks like. Jerks.

</div>I'm in favour of cloud storage over local tape storage because I think it is more reliable once the data is up there, easier to recover(no messy bringing tapes back on site), more durable (are you really going to have a tape drive around in 10 years time to read these tapes? One in working condition?) and generally easier. There is also a ton of fun stuff you can do with your online backup that you can't do locally. Consider building a mirror of your entire environment online. Having all your data online also lets you run analysis of how your data changes and who is making changes.

@HOVERBED suggested that having your backups available at all time is a security risk. What if people sneak in and corrupt them? I believe the same risk exists on tape. Physical security is largely more difficult than digital security. Most of the attacks on data are socially engineered or user error rather than software bugs allowing access. The risk can be mitigated by keeping checksums of everything an validating the data as you restore it.

Okay so now we're onto SANs and NASs(Is that how you pluralize NAS? Well it is now.). More expensive than disk in my workstation? Actually not anymore. The storage on my workstation is SSD which, despite price cuts, is more expensive than buying the same amount of storage on a SAN. But why are we still buying SANs? The beauty of a SAN is that it is a heavily redundant storage device which can easily be attached to by a wide variety of devices.

Enter ZFS. ZFS is a file system created by Sun back when they were still a good technology company. I'm not going to go too far into the features but ZFS allows you to offer many of the features of a SAN on more commodity hardware. If that isn't good enough then you can make use of a distributed file system to spread your data over a number of nodes. Distributed file systems can handle node failures by keeping multiple copies of files on different machines. You can think of it as RAID in the large.

To paraphrase networking expert Tony Stark:That's how Google did it, that's how Azure does it, and it's worked out pretty well so far.

Storage in this fashion is much cheaper than a SAN and still avoids much of the human factor. To expand a disk pool you just plug in new machines. Need to take machines offline? Just unplug them.

So is storage expensive? No. Is it more expensive than I think? Slightly. Am I going to spend the time trimming down my application? Nope, I spent the time writing this blog post instead. Efficiency!



