---
layout: post
title: Test Categories for MSTest
authorId: simon_timms
date: 2011-01-12
---

The version of MSTest which comes with Visual Studio 2010 has a new feature in it: test categories. These allow you to put your tests into different groups which can be configured to run or not run depending on your settings. In my case this was very handy for a specific test. Most of my database layer is mocked out and I run the tests against an in memory instance of SQLite. In the majority of cases this gives the correct results, however I had one test which required checking that values were persisted properly across database connections. This is problematic as the in memory SQLite database is destroyed on the close of a connection. There were other possible workarounds for this but I chose to just have that one test run against the actual MSSQL database. Normally you wouldn't want to do this but it is just one very small test and I'm prepared to hit the disk for it. I don't want this particular test to run on the CI server as it doesn't have the correct database configured.

In order to make use of a test category start by assigning one with the TestCategory attribute.

  
[TestMethod]  
[TestCategory("MSSQLTests")]  
public void ShouldPersistSagaDataOverReinit()  
{  
 FluentNhibernateDBSagaPersister sagaPersister = new FluentNhibernateDBSagaPersister(new Assembly[] { this.GetType().Assembly }, MsSqlConfiguration.MsSql2008.ConnectionString(ConfigurationManager.ConnectionStrings["sagaData"].ConnectionString), true);  
 ...buncha stuff...  
 Assert.AreEqual(data, newData);  
}

Next in your TFS build definition add a rule in the Category Filter box to exclude this category of tests.

[![](http://stimms.files.wordpress.com/2011/01/notmssql.png?w=300)](http://stimms.files.wordpress.com/2011/01/notmssql.png)

The [category field](http://msdn.microsoft.com/en-us/library/ms182489.aspx#category) has a few options and supports some simple logic.

That is pretty much it, good work!



