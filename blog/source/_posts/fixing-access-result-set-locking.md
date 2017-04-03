---
layout: post
title: Fixing Access Result Set Locking
authorId: simon_timms
date: 2013-05-17
---

If you're running an Access front end to a SQL database then there are frequently issues with table locking. From what I can tell when Access encounters a large table or view which is being used to populate a grid it will issue a query to retrieve the entire dataset using a cursor. It will then move the cursor along just far enough to see a bunch of records and hold the cursor open. This has the effect of locking a significant part of one or multiple tables. There are solutions and this blog post will take a look at a couple of them.

The first thing to do is try to identify the query which is causing the problem. In my case I found a reproducible test case in which an update query timed out. In a multi-user system this is the hardest part. I was aware the bug existed but with 40+ people in the database the problem only seemed to show up while I was at lunch and by the time I was back had cleared up. I finally narrowed it down to one screen and to verify it I ran

    select cmd,* from sys.sysprocesses where blocked > 0

This query finds all the executing processes on the SQL server which are blocked. The contents of the blocked field is actually the Id of the session which is preventing the query from going through. With that information in hand you can run

<script src='https://gist.github.com/stimms/5542028.js'></script>

with the session Id from the previous query. This will get you the currently running query which is blocking your update. Great, now you have a test case! If you want to know who is running the query you can look up the session Id against the output of

sp_who

But it doesn't really matter which user it is. Now to fix it.


# Fixing with Access

The problem we're having is that Access, in the interests of showing data quickly, is not pulling the entire data set. You can hook into the form open and have it jump to the end of the result set to have it pull all the records and close the cursor. In the subform with the locking query add this to the form load:

<script src='https://gist.github.com/stimms/5542087.js'></script>


# Fixing with SQL Server

The Access based solution isn't a great one because it can force Access to pull back an entire, huge, result set. This isn't performant. Okay, Access as a whole isn't performant but this is particularly bad. Instead we make sure that our form is based on a view instead of a table. If you've done anything with Access and SQL Server you're probably already doing this because of the crippling performance issue with crossing tables retrieved from SQL server. To get rid of the locking contention you can use the table hint with(NOLOCK) in your view definition.

If your view looks like, say

<script src='https://gist.github.com/stimms/5542393.js'></script>

and you're running into locking issues on tblTags you can change it to look like

<script src='https://gist.github.com/stimms/5542402.js'></script>

There is nothing stopping you from putting NOLOCK on every table in the view but it will increase the frequency of running into inconsistent behaviour. And you will run into inconsistent behaviour. NOLOCK is a terrible idea in general as it changes the isolation level to READ UNCOMMITTED. It is much better to not use cursors or sort out your transactions but such options are not open to us when using Access as it is a black box over which we have no control. There is [plenty](http://blogs.msdn.com/b/davidlean/archive/2009/04/06/sql-server-nolock-hint-other-poor-ideas.aspx) [written](http://sqlblogcasts.com/blogs/tonyrogerson/archive/2006/11/10/1280.aspx)out there about why NOLOCK is dangerous but it is the best of several bad options.

The real solution is to stop using Access. Please. Just stop.



