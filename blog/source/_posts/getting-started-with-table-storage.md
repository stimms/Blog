---
layout: post
title: Getting Started With Table Storage
authorId: simon_timms
date: 2013-04-10
---

In the [last post](http://blog.simontimms.com/2013/04/09/azure-table-storage/ "Azure TableStorage") we started looking at table storage and denormalization, but we didn't actually use table storage at all. The first thing to know is that your data objects need to extend

Microsoft.WindowsAzure.Storage.Table.TableEntity

This base class is required to provide the two key properties: partition and row key. These two keys are important foridentifyingeach record in the table. The row key provides a unique value within the partition. The partition key provides a hint to the table storage about where the table data should be stored. All entries which share a partition key will be on the same node where as different partition keys within the table may be stored on different nodes. As you might expect this provides for someinterestingquery optimizations.

If you have a large amount of data or frequent queries you may wish to use multiple partition keys to allow for loadbalancingqueries. If you're use to just slapping amonotonicallyincreasing number in for your ID you might want to rethink that strategy. The combination of knowing the partition key and the row key allows for very fast lookups. If you don't know the row key then a scan of the entire partition may need to be performed which isrelativelyslow. This plays well into our idea of denormalizing. If you commonly need to look up user information using e-mail addresses then you might want to set your partition key to be the e-mail domain and the row key to be the user name. In this way queries are fast and distrubuted. Picking keys is a non-trivial task.

A key does not need to be constructed from a single field either, you might want to build your keys byconcatenatingseveral fields together. A great example might be if you had a set of geocoded points of interest. You could build a row key by joining the latitude and longitude into a single field.

To actually get access to the table storage you just need to use the table client API provided in the latest Azure SDK.

<script src='https://gist.github.com/stimms/5351999.js'></script>

inserting records is simple. I use [automapper](https://github.com/AutoMapper/AutoMapper) to map between my domain objects and the table entities.

<script src='https://gist.github.com/stimms/5352067.js'></script>

It is actually the same process for updating an existing record.

A simple retrieve operation looks like

<script src='https://gist.github.com/stimms/5352047.js'></script>

Those are pretty much the basics of table storage. There is also support for a limited set of Linq operations against the table but for the most part they are ill advised because they fail to take full advantage of the key based architecture.



