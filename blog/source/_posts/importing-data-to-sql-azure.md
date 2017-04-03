---
layout: post
title: Importing On-Premise SQL to SQL Azure
authorId: simon_timms
date: 2015-01-15
---
Microsoft have done a great job building Azure and SQL Azure. However one of the places where I feel like they have fallen down is how to get your on premise data up into Azure. It isn't that there isn't a good way it is that there are a million different ways to do it. If you look at the official [MSDN entry](http://msdn.microsoft.com/en-us/library/azure/ee730904.aspx) there are 10 different approaches. How are we to know which one to use? I think we could realistically reduce the number to two methods:

1. Export and import a data-tier application
2. Synchronize with an on premise application

The first scenario is what you want in most instances. It will require downtime for your application as any data created between exporting your database and importing it will not be seen up in Azure SQL. It is a point in time migration. 

If you have a zero downtime requirement or your database is so large that it will take an appreciable period of time to export and import then you can use the [data sync](http://azure.microsoft.com/en-us/documentation/articles/sql-database-get-started-sql-data-sync/). This will synchronize your database to the cloud and you then simply need to switch over to using your up to date database in the cloud. 

This article is going to be about method #1. 

The first step is to find the database you want to export in SQL Server Management Studio

![Database selected](http://i.imgur.com/PMnYYVd.png)

Now under tasks select export data-tier application. This will create a .bacpac file that is portable to pretty much any SQL server.

![Select export data-tier application](http://i.imgur.com/ztBETDi.png)

Here you can export directly to a blob storage container on Azure. I've blanked out the storage account and container here for SECURITY. I just throw the backup in an existing container I have for moving data about. If you don't have a container just create a new storage account and put a container in it. 

![Imgur](http://i.imgur.com/j62uEh4.png)

The export may take some time depending on the size of your database but the nice part is that the bacpac file is uploaded to azure as part of the process so you can just fire and forget the export. 

Now jump over to the Azure management portal. I like to use the older portal for this as it provides some more fidelity when it comes to finding and selecting your bacpack file. Here you can create a new database from an export. Unfortunately there doesn't seem to be a way to apply an import to an existing database. That's probably not a very common use case anyway. 

![Create database](http://i.imgur.com/SxgyDQo.png)

Here we can specify the various settings for the new database. One thing I would suggest is to use a more performant database that you might usually. It will speed up the import and you can always scale down after. 

![Import settings](http://i.imgur.com/QSrs28o.png)

Now you just need to wait for the import to finish and you'll have a brand new database on Azure complete with all your data. 

![Import complete](http://i.imgur.com/G0HoS6V.png)

