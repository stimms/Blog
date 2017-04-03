---
layout: post
title: Experiments with Azure Event Hubs
authorId: simon_timms
date: 2014-08-19
---

A couple of weeks ago Microsoft released Azure Event Hub. These are another variation on service bus that go on to join queues and topics. Event Hubs are Microsoft's solution to ingesting a large number of messages from Internet of Things or from mobile devices or really from anything where you have a lot of devices that produce a lot of messages. They are prefect for sources like sensors that report data every couple of seconds.

There is always a scalabilitystory with Azure services. For instance with table storage there is a partition key; there is a limit to how much data you can read at once from a single partition but you can add many partitions. Thus when you're designing a solution using table storage you want to avoid having one partition which is particularly hot and instead spread the data out over many partitions. With Event Hubs the scalability mechanism is again partitions.

When sending messages to table storage you can pick one of n partitions to handle the message. The number of partitions is set at creation time and values seem to be in the 8-32 range but it is possible to go up to 1024. I'm not sure what real world metric the partition count maps to. At first I was thinking that you might map a partition to a device but with a maximum around 1024 this is clearly not the approach Microsoft had in mind. I could very easily have more than 1024 devices. I understand that you can have more than 1024 partitions but that is a contact support sort of operation. The messages within a partition are delivered to your consumers in order or receipt.

[![Event Hubs](http://stimms.files.wordpress.com/2014/08/event-hubs.png)](https://stimms.files.wordpress.com/2014/08/event-hubs.png)

In order delivery sounds mildly nifty but it is actually a huge technical accomplishment. In a distributed system doing anything in order is super difficult. Their cheat is that there is only a single consumer for each partition. I should, perhaps, say that there is at most one consumer per partition. Each consumer can handle several partitions. However you can have multiple consumer groups. Each consumer group gets its own copy of the message. So say you were processing alerts from a door open sensor and you want to send text messages when a door is opened and you want to log all open events in a log then you could have two consumers in two groups. Realistically you could probably handle both of these things in a single consumer but let's play along with keeping our microservices very micro.

<div class="wp-caption aligncenter" id="attachment_3395" style="width: 285px">[![An open closed sensor - this one is an Insteon sensor](http://stimms.files.wordpress.com/2014/08/open-closed.png)](https://stimms.files.wordpress.com/2014/08/open-closed.png)A magnetic open closed sensor â€“ this one is an Insteon sensor

</div>Messages sent to the event hub are actually kept around for at least 24 hours and can be configured up to 7 days. The consumers can request messages from any place in the stream history. This means that if you need to replay an event stream because of some failure you're set. This is very handy should you have a failure that wipes out some in memory cache (not that you should take that as a hint that the architecture I'm using leverages in memory storage).

Until now everything in this article has been discoverable from the rather sparse Event Hub documentation. I had a bunch more questions about the provided EventProcessorHost that needed answering. EventProcessorHost is the provided tool for consuming message. You can consume messages using your own connectors or via EventHubReceiver but EventProcessorHost provides some help for dealing with which node is responsible for which partitions. So I did some experiments

**What's the deal with needing blob storage?**

It looks like theEventProcessorHost writes out timestamps and partition information to the blob storage account. Using this information it can tell if a processing node has disappeared requiring it to spread the lost responsibility over more nodes. I'm not sure what happens in event of a network partition. It is a bit involvedto test. The blob storage is checked every 10 seconds so you could have messages going unprocessed for as long as 20 seconds.

Opening up the blog storage there is a blob for each consumer group * each partition. So for my example with only the $Default group and 16 partitions there were 16 blobs. Each one contained some variation of

{"PartitionId":"10","Owner":"host1","Token":"87f0fe0a-28df-4424-b135-073c3d007912","Epoch":3,"Offset":"400"}

**Is processing on a single partition single-threaded?**

Yes, it appears to be. This is great, I was worried I'd have to lock each partition so that I didn't have more than one message being consumed at a time. If that were the case it would sort of invalidate all the work done to ensure in order delivery.

**Is processing multiple messages on different partitions on a single consumer multi-threaded?**

Yes, you can make use of threads and multiple processors by having one consumer handle several partitions.

**If you register a new consumer group does it have access to messages published before it existed?**

I have no idea. In theory it should but I haven't been able to figure out how to create a non-default consumer group. Or, more accurately, I haven't been able to figure out how to get any messages for the non-default consumer group. I've asked around but nothing so far. I'll update this if I hear back.



