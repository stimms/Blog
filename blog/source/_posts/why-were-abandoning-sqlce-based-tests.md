---
layout: post
title: Why we're abandoning SQLCE based tests
authorId: simon_timms
date: 2012-07-17
---

For a while now we've been using either in memory SQLite or SQLCE databases for our integration tests. It is all pretty simple we spin up a new instance of the database, load in a part of our schema, populate it with a few test records and run our repositories against it. The database is destroyed and created for each test. The in memory version of SQLite was particularly quick and we could run our tests in a few seconds.

The problem was that the dialect of SQL that SQLite and SQLCE speak is slightly different from the dialect that SQL Server 2008 speaks. This wasn't a big problem when we were building our schemas through NHibernate as it supports outputting tables and queries in a variety of different dialects. In our latest project we move to using Dapper. Instead of creating our schema through a series of fluent configurations as we did with NHibernate we used a database project. This allowed us much finer grained control over what was created in the database but it meant that we were more closely tied to the database technology.

For a while we wrote transformations for our create table scripts and queries which would translate them into SQLCE's version of TSQL. This code grew more and more complicated and eventually we realized that we were trading developer time for computer time, which is a terrible idea. Computers are cheap and developers are expensive. The class of tests we were writing weren't unit tests they were integration tests so they could take longer to run without having a large impact on development. We were also testing against a database which wouldn't be used in production. We could have all sorts ofsubtlebugs in our SQL and we would never know about it, at least not until it was too late.

Instead we've started mounting a database on a real version of SQL Server 2008 R2, the same thing we're using in production. The tests do indeed run slower as each one pays a few seconds of start up cost more than it did with the in memory database. We consider the move to be a really good one as it gets us closer to having reliable tests of the entire stack.



