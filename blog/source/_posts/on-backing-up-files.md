---
layout: post
title: On Backing up Files
authorId: simon_timms
date: 2011-01-02
---

I've been meaning to set up some backups for ages and at last today I got around to it. I have a bunch of computers in the house and they kind of back each other up but if my house goes up in smoke or if some sort of subterranean mole creates invade Alberta in revenge for the oil sands I'm out of luck.

I really like this company [BackBlaze](http://www.backblaze.com/) who offer infinite backups for $5 a month. I have some issues with them which I've outlined in another blog post which is, perhaps, a bit too vitriolic to post. Anyway they have little client which runs on OSX or Windows and steams files up to their data center. What they lack is support for UNIX operating systems and I wanted a consistent backup solution for all my computers so something platform portable was required. They also had some pretty serious downtime a while ago which I felt highlighted some architectural immaturity.

My next stop was this rather nifty [S3 BackupSystem](http://www.s3backupsystem.com/). Again this was windows only and couldn't backup network drives so it was out.

Finally I came acorss [s3sync.rb](http://s3sync.net) a ruby script which was designed to work like rsync. It is simple to use and can be shoved into cron with narry a thought. At its most basic you need to do:

**Set up Keys**

<div style="background-color:#f3f3f3;">export AWS_ACCESS_KEY_ID=   
 export AWS_SECRET_ACCESS_KEY_ID= </div>or set the variables in

<div style="background-color:#f3f3f3;">$HOME/.s3conf/s3config.yml</div>**Backup /etc to S3**

<div style="background-color:#f3f3f3;">s3sync.rb -r /etc :</div>**Restore /etc from S3**

<div style="background-color:#f3f3f3;">s3sync.rb -r : /etc</div>This assumes you have set up S3 already. If not head over to [Amazon](http://aws-portal.amazon.com/gp/aws/developer/subscription/index.html?productCode=AmazonS3) and set one up. When you sign up take note of the keys they give out during the sign up. If you fail to do so they can still be accessed once the account is set up from Security Credentials under your account settings. S3 is pretty cheap and I think you even get 15gig for free now. If that ins't enough then it is still something like 15 cents a gig/month in storage and 10 cents a gig in transfer. Trivial to say the least.



