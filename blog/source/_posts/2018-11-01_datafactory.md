layout: post
title: Azure Data Factory - a rapid introduction
authorId: simon_timms
date: 2018-11-01
---

Azure is huge. There are probably a dozen ways to host a website, a similar number of different data storage technologies, tools for identity, scaling, DDoS protection - you name it Azure has it. With that many services it isn't unusual for me to find some service I didn't even know existed. Today that service is [Data Factory](https://azure.microsoft.com/en-ca/services/data-factory/). Data factory is a batch based Extract, Transform and Load(ETL) service which means that it moves data between locations. I mention that it is batch to distinguish it from services which are online and process events as they come in. Data Factory might be used to move data between a production database and the test system or between two data sources. 

<!--more-->

In most cases I'd recommend solutions which were more tightly integrated into your business processes than copying data between databases. It is easier to test, closer to real time and easier to update. However, moving to event based systems can be long and difficult so there is certainly a niche for Data Factory. A good application might be migrating data from a service you don't own to your internal database - the external system is unlikely to have data change notifications you could use to drive data population. Azure Data Factory plays in the same space that SQL Server Integration Services played in the past - in fact you can build your Azure Data Factory pipeline in SSIS and simply upload it.

Let's take a look at loading some data from an Azure SQL database, manipulating it a little bit and putting it into Cosmos DB. I've chosen these two systems but there are literally dozens of different data sources and destinations you can use at the click of a mouse. They are not limited to Microsoft offerings, either. There are connectors for Cassandra, Couchbase, MongoDB, Google BigQuery even Oracle. 

![/images/datafactory/datasources.png](/images/datafactory/datasources.png)