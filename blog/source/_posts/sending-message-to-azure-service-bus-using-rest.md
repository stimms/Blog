---
layout: post
title: Sending Message to Azure Service Bus Using REST
authorId: simon_timms
date: 2015-01-30
---
Geeze, that is a long blog title. 

In this post I'm going to explore how to send messages to Azure service bus using the HTTPS end point. HTTP has become pretty much a lingua franca when it comes to sending messages. While it is a good thing in that most every platform has a web client HTTP is not necessarily a great protocol for this. There is quite a bit of overhead some from HTTP itself and some from TCP at least we're not sending important messages over UDP. 

In my case here I have an application running on a device in the field (that's what we in the oil industry call anything that isn't in our nice offices). The device is running a full version of Windows but the application from which we want to send messages is running an odd sort of programming language that doesn't have an Azure client built for it. No worries, it does have an HTTP Client. 

The first step is to set up a queue to which you want to send your messages. This has to be done from the old portal. I created a namespace and in that I created a single queue. 

![Portal](http://i.imgur.com/tC7e2Hz.png)

Next we add a new queue and in the configuration for this queue we add a couple of access policies:

![Access policies](http://i.imgur.com/XqIRTFQ.png)

I like to add one for each of the combinations of services. I don't like a single role that isn't the manager to be able to send and listen on a queue. It is just the principle of least privilege in action. 
Now we would like to send a message to this queue using REST. There are a couple of ways of getting this done. The way I knew about was to generates a WRAP token by talking to the access control server. However as of August 2014 the ACS namespace is not generated by default for new service bus name spaces. I was pointed to [an article](http://blogs.msdn.com/b/cie/archive/2014/08/29/service-bus-namespace-creation-on-portal-no-longer-has-acs-connection-string.aspx) about it by my good buddy [Alexandre Brisebois]( http://alexandrebrisebois.com)*. He also recommended using Shared Access Signatures instead. 

A shared access signature is a token that is generated from the access key and an expiry date. I've seen these used to grant limited access to a blob in blob storage but didn't realize that there was support for them in service bus. These tokens can be set to expire quite quickly meaning that even if they fall into the hands of an evil doer they're only valid for a short window which has likely already passed. 

Generating one of these for service bus is really simple. The format looks like 

    SharedAccessSignature sr={0}&sig={1}&se={2}&skn={3}

- 0 is the URL encoded queue end point (https://ultraawesome.servicebus.windows.net/awesomequeue)
- 1 is the URL encoded signature created by

```
[HMACSHA256 using key of [[queue address] + [new line] + [expiry date in seconds since epoch]]]
```
- 2 is the expiry, again in seconds since epoch
- 3 is the key name, in our case Send

This will generate something that looks like

```
SharedAccessSignature sr=https%3a%2f%2fultraawesome.servicebus.windows.net%2fawesomequeue%2f&sig=WuIKwkBuB%2fjxMgK6x79o3Xrf4nKZtWX9defu7HLdzWg%3d&se=1422636195&skn=Sen
d
```

With that in place I can now drop to CURL and attempt to send a message

```
curl -X POST https://ultraawesome.servicebus.windows.net/awesomequeue/messages -H "Authorization: SharedAccessSignature sr=https%3a%2f%2fultraawesome.servicebus.windows.net%2fawesomequeue%2f&sig=WuIKwkBuB%2fjxMgK6x79o3Xrf4nKZtWX9defu7HLdzWg%3d&se=1422636195&skn=Send" -H "Conent-Type:application/json" -d "I am a message"
```
This works like a dream and in the portal we can see the message count tick up. 

In our application we need only generates the shared access token and we can send the message. If the environment is lacking the ability to do HMACSHA256 then we could call out to another application or even pre-share a key with a long expiry, although that would invalidate the advantages of time locking the keys. 

*We actually only met once at the MVP summit but I feel like we're brothers in arms. 



