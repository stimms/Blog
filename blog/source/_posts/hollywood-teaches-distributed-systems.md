---
layout: post
title: Hollywood Teaches Distributed Systems
authorId: simon_timms
date: 2013-04-08
---

I'm sitting here watching the movie Battle: LA and thinking about how it relates to distributed systems.The movie is available on Netflix if you haven't seen it. You should stop reading now if you don't want me spoiling it.

The movie takes place, unsurprisingly, in Los Angeles where an alien attack force is invading the city. As Aaron Eckhart and his band of marines struggle to take back the city they discover that the ships the aliens are using are actuallyunmanneddrones.

<div class="wp-caption aligncenter" id="attachment_2566" style="width: 651px">[![War Machine](http://stimms.files.wordpress.com/2013/04/battlelosangeles.jpg)](http://stimms.files.wordpress.com/2013/04/battlelosangeles.jpg)Unmanned, except for the man in front, obviously

</div>This is not news the member of the signal corps they have hooked up with. She believes that all the drones are being controlled from a central location, a command and control center. The rest of the movie follows the marines as they attempt to find this structure which, as it turns out, isburiedin the ground. It is, seemingly, very secret and very secure hidden in the ruined city.Fortunately, as seems to happen frequently in these sorts of movies, the US military prevails and blows up this centralstructure.

The destruction of the command ship causes all the drones, which where were previously holding human forces at bay, to crash and explode. It is a total failure as a distributed system.  Destroying one central node had the effect of taking out a whole fleet of automated ships. The invasion failed because some tentacled alien failed to read a book on distributed systems.

See the key is that you never know what is going to fail. Having a single point of failure is a huge weakness. Most people, when they're designing systems don't even realize that what they've got is a distributed system. I've seen this costing a lot of people a lot of time a couple of times. I've seen lone SAN failure take out an entire company's ability to work. I've seen failures in data centers on the other side of the planet take out citrix here. If there is one truth to the information systems in large companies it is that they are complicated. However the people working on them frequently fail to realize that what they have on their hands is a single, large, distributed system.

For sure some services are distributed by default (Active Directory, DNS,â€¦) but many are not. Think about file systems: most companies have files shared from a single point, a single server. Certainly that server might have multiple disks in a RAID but the server itself is still a single point of failure. This is why it's important to invest in technologies like Microsoft's Distributed File System which uses replication to ensure availability. Storage is generally cheap, much cheaper than dealing with downtime from a failed node in Austin.

Everything is a distributed system, let's start treating it that way.



