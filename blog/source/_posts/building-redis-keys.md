---
layout: post
title: Building Redis keys
authorId: simon_timms
date: 2014-01-27
---

I was having a discussion with somebody the other day and wanted to link them to an article on how you can use compound names as effective keys in key value stores like Redis. Weirdly I couldn't find a good article on it. Perhaps my google-fu (actually duck-duck-go-fu now) isn't as strong as I thought or perhaps there isn't an article. In either case this is now going to be an article on how to build keys for KV stores.

KV-stores are one of the famed NOSQL databases. There are quite a few options for [key value stores](https://en.wikipedia.org/wiki/NoSQL#Key.E2.80.93value_stores)these days, I like Redis and I've also made use of Azure's blob storage which is basically a key value store. KV-stores are interesting in that they are highly optimized for looking things up by a known key. Unlike relational databases you can't easily query against the values. This means that there is no doing

select * from users where name like 'John%'

to find all the users with a first name of John. This isn't the sort of operation for which key value stores are good. You can list keys in Redis by using the KEYS command but it is generally not encouraged as it isn't performant. You really need to know the key for which you're looking.

The KV key can be stored in another storage system like a relational database. Perhaps you might store several indexes of keys in the relational database, filter and retrieve sets of them and then pull the records from the KV-store. So say you have a list of trees you want to store. In the relational database you can create a table of tree types (coniferous, deciduous, carnivorous) and the Redis keys. You also store the maximum heights and keys in the relational database. The details of the trees go into Redis for rapid retrieval. First query against the relational database then against the KV store.

Sounds unnecessarily complicated? Yeah, I think so too. The better approach is to use a key which can be constructed without need to query another data store explicitly. If we had a database of historical values of temperatures in all of North America then we could build our keys to make them match our data access pattern. The users for this database are most interested in looking up values for their hometown on specific days. Thus we've constructed our keys to look like

<country>:<state/province>:<city>:<year>:<month>:<day>

This allows us to build up a key programmatically from the user's query

GET US:NewYork:Albany:1986:01:15 -> -8

We can be even smarter and build keys like

<country>:<state/province>:<city>:<year>:<month>:<day>:Max <country>:<state/province>:<city>:<year>:<month>:<day>:Min

For more detailed information. They take away here is that you can build large and complex keys based on hierarchical data to make your lookups run in O(1) time, which is also known as O(jolly quick) which is a good place to be. Creating effective keys requires that you know about your access patterns ahead of time but that's true of most databases, even in a relational database you have to pick your indexes based on access patterns.



