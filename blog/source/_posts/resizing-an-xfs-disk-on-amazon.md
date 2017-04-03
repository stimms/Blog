---
layout: post
title: Resizing an XFS disk on amazon
authorId: simon_timms
date: 2014-01-13
---

I have a linux server I keep around on amazon EC2 for when I need to do linuxy thing and don't want to do them on one of my local linux boxes. I ran out of disk space for my home directory on this server. I could have mounted a new disk and moved /home to it but instead I thought I might give resizing the disk a try. Afterall it is 2014 and I'm sure resizing disks has long since been solved.

To start with I took the instance down by clicking on stop in the amazon console. Next I took a snapshot of the current disk and named it something I could easily remember.

[![Screen Shot 2014-01-11 at 7.45.08 PM](http://stimms.files.wordpress.com/2014/01/screen-shot-2014-01-11-at-7-45-08-pm.jpg?w=750)](http://stimms.files.wordpress.com/2014/01/screen-shot-2014-01-11-at-7-45-08-pm.jpg)This snapshot works simply as a backup of the current state of the disk. The next step was to build a new volume and restore the snapshot onto it.

[![Screen Shot 2014-01-11 at 8.04.58 PM](http://stimms.files.wordpress.com/2014/01/screen-shot-2014-01-11-at-8-04-58-pm.jpg?w=750)](http://stimms.files.wordpress.com/2014/01/screen-shot-2014-01-11-at-8-04-58-pm.jpg)I made the new volume much larger and selected the snapshot I had just created as the base for it. The creation process took quite some time, a good 10 minutes, which was surprising as the disk wasn't all that big. Now I had two identical volumes except one was bigger. As I only have one instance on EC2 I attached the volume back to my current instance as /dev/sdb1. I started the instance back up and sshed in.

To actually resize the file system turned out to be stunningly easy, although I had a few false starts because I thought I was running ext3 and not xfs. You can check this by catting out /etc/mtab, the third column will be the file system type.

The existing volume and the snapshot volume will have the same UUID so this must be updated by running.

sudo xfs_admin -U generate /dev/sdb1

Now you can mount the new volume without error

sudo mount /dev/sdb1 /mnt

Then simply run

sudo xfs_growfs /dev/sdb1

This will grow the xfs volume to fill the whole volume. I then shut the machine down, switched the two disks around and rebooted. I made sure the system was working fine and had sufficient storage. As it did I deleted the old volume and the snapshot.



