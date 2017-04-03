---
layout: post
title: A Spike on SQL Server Backups
authorId: simon_timms
date: 2013-04-18
---

I frequently feel that if there is a hole in my knowledge of my development stack it is SQL Server. Honestly the thing is a mystery to me for the most part. I put data in it and data comes out. From time to time I add an index and data comes out faster. Sometimes I look at query plans but mostly I do that in front of other developers so they believe that I know SQL Server. I tell you looking at a query plan then tapping your monitor with a pen and going â€œyep, yepâ€ gives you a lot of street cred in development shops.

<div class="wp-caption aligncenter" id="attachment_2612" style="width: 510px">[![Street Cred](http://stimms.files.wordpress.com/2013/04/image001-2.jpg)](http://stimms.files.wordpress.com/2013/04/image001-2.jpg)Street Cred

</div>But my lack of understanding of SQL Server backups ends this day!

Okay so the first thing is that there are 3 different recovery modes for SQL Server: Simple, Full and Bulk logged.

**Simple -**In this mode no log files are backed up only a snapshot of the data. This mode means that there is no need to keep transaction log files around. However if you lose the database then you also lose all the data since your last backup. Depending on your use case this might be okay but I doubt it. I would, personally, stay away from simple recovery mode.

**Full** â€“ The log file is backed up in this version which means that you can recover up to the last transaction. In addition you can stop the restore at any point, essentially giving you a snapshot of the database at any point in time. That's pretty awesome for many applications which are not even disaster recovery related.

**Bulk logged -**This is a special case of full backups which allows for better performance by not logging the details of certain bulk operations. For the most part this mode is as recoverable as the previous but saves you space if you make use of a lot of bulk operations. You probably shouldn't make use of a lot of bulk operations anyway.

Great that clears things up, except: what's a transaction log?

A transaction log is an append only file which contains a record of every transaction, or change, which occurs on a SQL server. Information is written during the lifetime of a transaction which means that if the sever crashes it can finish up or roll back half finished transactions using the log. Replaying a full log can restore a database. The log can also be shipped to other instances which can replay the log to bring themselves up to date. You might want to do that if you have a number of read only mirrors of your database for scalability or high availability purposes.

As you might imagine these log files can grow out of control pretty quickly.Fortunatelyyou can truncate them from time to time. From what I'm reading the log file is truncated as soon as it has been backed up. However the log file may not actually be reduced in size. This is because allocating disk is expensive so SQL server keeps hold of what is, in effect, an empty file. If you really need the space back you can run

DBCC SHRINKFILE ('YourDataBase_Log', 1000)

There are also some corner cases where the log file will not be truncated during a backup â€“ such as when a restore is running using that transaction log.

The log file should be stored on a different disk from the mdf file. And when I say a different disk I mean a different storage systementirely. So if the mdf is on a SAN then the log file shouldn't be on that SAN unless you're willing to lose data should the entire SAN fail(hey, it happens).



