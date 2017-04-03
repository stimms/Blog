---
layout: post
title: MySQL in an NServiceBus Handler
authorId: simon_timms
date: 2010-10-05
---

I have an autonomous component in my NServiceBus implementation which needs to talk to a MySQL database. When I first implemented the handler I configured the end point to be transactional, mostly because I wasn't too sure about the difference between configuring AsA_Service and AsA_Client and transactions sounded like they might be something I would like. What the transactional endpoint does is wrap the endpoint in a distributed transaction. A distributed transaction is a mechanism which allows you to share a transaction between a number of databases so that if you're writing to several databases when you commit to one database and it fails then the transactions in the other databases will rollback.

However when I went to test the handler it failed with an error:

<span style="font-style:italic;">MySql /Net connector does not support distributed transactions</span>

I solved it by configuring the endpoint AsA_Client. The problem with that configuration is that the handler wipes the queue on startup which isn't ideal. None of the built in configurations are quite right for our situation so we override the configuration as described here: http://www.nservicebus.com/GenericHost.aspx.



