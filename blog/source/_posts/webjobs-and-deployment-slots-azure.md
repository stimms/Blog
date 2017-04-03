---
layout: post
title: WebJobs and Deployment Slots (Azure)
authorId: simon_timms
date: 2015-03-25
---
I should start this post by apologizing for getting terminology wrong. Microsoft just renamed a bunch of stuff around Azure WebSites/Web Apps so I'll likely mix up terms from the old ontology with the new ontology (check it, I used "ontology" in a sentence, twice!). I will try to favour the new terminology. 

On my Web App I have a WebJob that does some background processing of some random tasks. I also use scheduler to drop messages onto a queue to do periodic tasks such as nightly reporting. Recently I added a deployment slot to the Web App to provide a more seamless experience to my users when I deploy to production, which I do a few times a day. The relationship between WebJobs and deployment slots is super confusing in my mind. I played with it for an hour today and I _think_ I understand how it works. This post is an attempt to explain. 

**If you have a deployment slot with a webjob and a live site with a webjob are both running?**

Yes, both of these two jobs will be running at the same time. When you deploy to the deployment slot the webjob there is updated and restarted to take advantage of any new functionality that might have been deployed. 

**My job uses a queue, does this mean that there are competing consumers any time I have a webjob?**

If you have used the typical way of getting messages from a queue in a webjob, that is to say using the QueueTrigger annotation on a parameter:

```
public static void ProcessQueueMessage([QueueTrigger("somequeue")] string messageText, TextWriter log)
{...}
```

then yes. Both of your webjobs will attempt to read this message. Which ones gets it? Who knows!

**Doesn't that kind of break your functionality if you're deploying different functionality for the same queue giving you a mix of old and new functionality?**

Yep! Messages might even be processed by both. That can happen in normal operation on multiple nodes anyway which is why your jobs should be idempotent. You can either turn off the webjob for your slot or use differently named queues for production and your slot. This can then be configured using the new [slot app settings](http://azure.microsoft.com/en-us/documentation/articles/web-sites-staged-publishing/#configuration-for-deployment-slots). To do this you need to set up a QueueNameResolver, you can read about that [here](http://azure.microsoft.com/en-us/documentation/articles/websites-dotnet-webjobs-sdk-storage-queues-how-to/#config) 

**What about the webjobs dashboard, will that help me distinguish what was run?**

Kind of. As far as I can tell the output part of this page shows output from the single instance of the webjob running on the current slot.

![Imgur](http://i.imgur.com/3kv54yI.png)

However the functions invoked list shows all invocations across any instance. So the log messages might tell you one thing and the function list another. Be warned that whey you swap a slot the output flips from one site to another. So if I did a swap on this dashboard and then refreshed the output would be different but the functions invoked list would be the same.

