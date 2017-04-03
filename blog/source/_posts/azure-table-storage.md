---
layout: post
title: Azure Table Storage
authorId: simon_timms
date: 2013-04-09
---

Azure table storage is another data persistence open when building applications on Azure. If you're use to SQL Server or another pure SQL storage solution then table storage isn't all that different. There is still a concept of a table made up of columns and row. What you lose is the ability to join tables. This makes for some interesting architectural patterns but it actually ends up being not that big of a leap.

We have been conditioned to believe that databases should be designed such that data is well partitioned into its own tiny corner. We build a whole raft of table each one representing bit of data which we don't want to duplicate. Need an address for your users? Well that goes into the address table. As we continue down that line we typically end up with something like this:

<div class="wp-caption aligncenter" id="attachment_2580" style="width: 760px">[![Typical Relational Database Structure](http://stimms.files.wordpress.com/2013/04/screen-shot-2013-04-08-at-9-57-25-pm.jpg)](http://stimms.files.wordpress.com/2013/04/screen-shot-2013-04-08-at-9-57-25-pm.jpg)Typical Relational Database Structure

</div>Here each entity is give its own table. This example might be a bit contrived, I hope. But I'm sure we've seen databases which are overly normalized like this. Gosh knows that I've created them in the past. How to arrange data is still one of the great unsolved problems of software engineering as far as I'm concerned(hey, there is a blog topic, right there).

With table storage you would have two options:

1. Store the IDs just as you would in a fully relational database and retrieve the data in a series of operations.
2. Denormalize the database

I like number 2. Storing IDs is fine but really it adds complexity to your retrieval and storage operations. If you use table storage like that then you're not really using it properly, in my mind. It is better to denormalize your data. I know this is a scary concept to many because of the chances that data updates will be missed. However if you maintain a rigorousapproach to updating and creating data this concern is largely minimized.

<div class="wp-caption aligncenter" id="attachment_2581" style="width: 276px">[![Denormalized](http://stimms.files.wordpress.com/2013/04/screen-shot-2013-04-08-at-10-38-15-pm.jpg)](http://stimms.files.wordpress.com/2013/04/screen-shot-2013-04-08-at-10-38-15-pm.jpg)Denormalized

</div>To do so it is best to centralized your data persistence logic in a single area of code. In CQRS parlance this might be called a denormalizer or a disruptor in LMAX. If you have confidence that you can capture all the sources of change then you should have confidence that your denormalized views have correct data in them. By directing all change through a central location you can build that confidence.

In tomorrow's post I'll show how to make use of table storage in your web application.



