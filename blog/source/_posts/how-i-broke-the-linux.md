---
layout: post
title: How I broke the Linux
authorId: simon_timms
date: 2013-07-30
---

Years ago I was big into the Linux. Heck I was big into all Unix stuff. I had a 3 node cluster of Solaris 9 servers in my basement once which, having been built from old hardware, was probably slower than any other single machine on my network. But then I got tired of screwing around with Linux and FreeBSD and OpenBSD and (I was young, I swear) OpenVMS. I got old and I just wanted things to work. If I buy a new video card I don't want to recompile my fricking kernel from sources I downloaded using and FTP client I wrote myself based on an argument I had with RMS in which I accused him of being a Microsoft shrill. Just work, damn it.

That being said I keep a few Linux boxes around to do things like serve files and do DHCP and the such. It was one of these boxes I rebooted after some updates last week. Now this box is amazingly stable and I have it on a UPS so its uptime was over 500 days. When it came back up a drive was missing. "That's weird" I thought and dug into it. This is my primary drive which contains terabytes of completely legally obtained videos. In a fit of anti-police sentiment I had encrypted the snot out of the drive with [TrueCrypt](http://www.truecrypt.org/). I had no idea what the passphrase was. I just remembered it was long. Like mindbogglinly long. So long that, were he still alive, Robert Jordan would be impressed. This drive was never going to be cracked and I didn't have the passphrase.

Well shoot.

So I decided I would throw the whole thing out and start over. All my important files were backed up to CrashPlan using a key I actually remembered. I would reformat and start over. A fresh start! I could get higher res versions of the stuff I had lost. Name them sequentially. It would be gloriously well ordered. Then I made my second mistake. I decided to upgrade the OS to the latest while I was in there.

Turns out that when I set up that machine I had used a software RAID1. It had, of course, never really worked properly. During boot md (the software RAID) would complain about being degraded. It never really seemed to be a big deal and I had run out of time when setting it up so I had left it. Turns out that one of the changes in the new version of the OS was to make this warning a [fatal error](https://bugs.launchpad.net/ubuntu/+source/mdadm/+bug/872220). Now the system won't boot.

I get dropped into a recovery console and I sigh. Fortunatly I still had some memory of how Linux works so I started to debug

>dmesg | less less: command not found >god damn it you stupid recovery shell god: command not found >tail dmesg

The final couple of lines of the output pointed to md as the culprit. I was going to rebuild the array anyway so I moved mdadm.conf out of /etc/mdadm to a backup so they system wouldn't try to mount any md drives. However, as it turns out that does nothing now in Ubuntu. Since I stopped knowing about Linux they seem to have created an init ram disk into which a subset of files is loaded. I have no recollection of this existing so it may be new or I may have just never run into it before. Anyway it holds a protected, secret copy of your mdadm.conf file so you can change the one in /etc forever and your system still won't boot. I call this the "you stupid newb" ram disk.

By this point I'd discovered that you could append

bootdegraded=true

to the kernel line to at least get a system up with a degraded array. I did that and managed to get into the system long enough to delete the array

mdadm --stop /dev/md0 mdadm --zero-superblock /dev/sd[bc]1

create a new array(RAID 0 this time)

mdadm --create /dev/md0 --level=0 --raid-devices=2 /dev/sdb1 /dev/sdc1

Update the mdadm.conf file

head -n -1 /etc/mdadm/mdadm.conf > /etc/mdadm/mdadm.conf.new mdadm --detail --scan >> /etc/mdadm/mdadm.conf.new mv/etc/mdadm/mdadm.conf.new/etc/mdadm/mdadm.conf

and set the init ram disk back up

update-initramfs -u

I rebooted and found everything to be in working order. Thank goodness.

So the lesson here is don't touch anything which is working. Don't touch it ever or you will break it and have to spend all evening fixing it instead of churning butter. Which would have been more fun.

<div class="wp-caption aligncenter" id="attachment_2955" style="width: 689px">[![Fun riot!](http://stimms.files.wordpress.com/2013/07/act_churning_butter.jpg?w=679)](http://stimms.files.wordpress.com/2013/07/act_churning_butter.jpg)Fun riot!

</div>

