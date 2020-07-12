---
layout: post
title: Azure Functions for NServiceBus
authorId: simon_timms
date: 2020-07-11 10:00

---

A while back I blogged about [Durable Functions vs. NServiceBus Sagas](/2019/02/14/2019-02-14-durablefunctions_sagas/). At the time I had some good chats with Sean Feldman and one of the ideas he tossed around was the possibility of running NServiceBus on top of Azure Functions. At the time I snorted at the idea because starting up NServiceBus is kind of a heavy weight process. There is assembly scanning and DI set up to do which, in my experience, takes a good second or two. It wouldn't be cost effective to run all that startup code on Functions. But Sean set out to prove me wrong. 

<!--more-->

So here we are some time later and NServiceBus are looking to release the first version of NServiceBus that runs on top of Functions. Let's talk about why this is exciting then take a look at how it looks and finally kick the tyres a little bit. 

# Why does this matter?

A little bit of background here: NServiceBus is backed by the company Particular. I tend to refer to the company as NServiceBus because that is their only product, but I'm not correct to do so. Anyway Particular are a pretty conservative company. They're going going to be chasing after the latest, coolest thing. This means that they're providing good stable software which (other than the NSB 5 -> NSB 6 transitions) doesn't change all that much. 

For an enterprise customer that's good news. You don't want to have to rewrite an ERP every 2 years when the technology changes. However I've been heard to complain that they're too conservative. MSMQ is not only still supported but was the default transport up until pretty recently. The licensing has also been targeted towards a static number of servers meaning that scaling up and down is difficult. Again, that's changed recently to be based on the number of logical endpoints. 

Azure Functions bring the ability to rapidly scale up NServiceBus endpoints, they can event scale transparently based on load and queue length. Couple this with the new logical endpoint billing and you've got yourself a way to scale out NServiceBus endpoints very quickly to handle inconsistent load. If startup time is reduced then this can be pretty cost effective too. 

# What's that looks like?

Okay let's look at the code for running a servicebus based NServiceBus endpoint on Functions. If we start with a new functions project the first thing we need is to pull in the appropriate packages: `NServiceBus.AzureFunctions.ServiceBus` and `NServiceBus.Newtonsoft.Json`. The former holds the bits of NServiceBus we need for this and the latter is just the serialization choice. 

Next we need a handler to handle some message. I like to imagine that I have a robot in the house dedicated to keeping me happy so we'll send control messages to this robot. 

```
using NServiceBus;

public class FetchCandyCommand : IMessage
{
    
}
```

That message could have attributes on it but let's assume our robot contains a candy selection AI (CSAI) so the command can remain pretty much empty.

The handler looks like 

```
using System.Threading.Tasks;
using NServiceBus;
using NServiceBus.Logging;

public class TriggerMessageHandler : IHandleMessages<FetchCandyCommand>
{
    private static readonly ILog Log = LogManager.GetLogger<TriggerMessageHandler>();

    public Task Handle(FetchCandyCommand message, IMessageHandlerContext context)
    {
        Log.Warn($"You're going to get fatter!");
        //fetch the candy
        return Task.CompletedTask;
    }
}
```

This looks just like a regular NSB handler, so it should be pretty easy to port any existing handlers you have over to using this functionality. 

Finally we need to do a little bit of wiring to get Functions and NServiceBus to talk to each other.

```
using Microsoft.Azure.ServiceBus;
using Microsoft.Azure.WebJobs;
using Microsoft.Extensions.Logging;
using NServiceBus;
using System.Threading.Tasks;
using NServiceBus.AzureFunctions.ServiceBus;

public class AzureServiceBusTriggerFunction
{
    private const string EndpointName = "RobotControlQueue";

    [FunctionName(EndpointName)]
    public static async Task Run(
        [ServiceBusTrigger(queueName: EndpointName)]
        Message message,
        ILogger logger,
        ExecutionContext executionContext)
    {
        await endpoint.Process(message, executionContext, logger);
    }

    private static readonly FunctionEndpoint endpoint = new FunctionEndpoint(executionContext =>
    {
        var configuration = new ServiceBusTriggeredEndpointConfiguration(EndpointName);
        configuration.UseSerialization<NewtonsoftSerializer>();

        configuration.AdvancedConfiguration.CustomDiagnosticsWriter(diagnostics =>
        {
            executionContext.Logger.LogInformation(diagnostics);
            return Task.CompletedTask;
        });

        return configuration;
    });
}
```

The `Run` function here is where Azure Functions will enter the function. You can see it has the `ServiceBusTrigger` annotation on it meaning that it will listen in on the `RobotControlQueue`. Once executed it will create a new `FunctionEndpoint` which is the NServiceBus entry point and is where the magic happens. This is where any setup is done for instance adding the serialization and setting up logging. 

There are a lot more input bindings in Azure Functions than just service bus. You could also kick off NServiceBus processing from something expected like storage queues. However you could also kick off processing from timer triggers or Microsoft Graph Events. What's perhaps more intriguing is that you can hook into the output bindings making it really easy to integrate with other Azure services while also being able to maintain an investment in NServiceBus. 

# Tyre Kicking

**Note:** I based everything I did off the sample project that Sean provided [here](https://docs.particular.net/samples/azure/functions/service-bus/).

In order to test this out you'll need to stand up a quick ServiceBus namespace. You'll also need to add a queue because Azure Functions doesn't stand one up by itself. With that all done it is just a question of running the function and watching the output. 

![Console showing the running app processing messages](/images/nsbfunctions/console.png)

The first execution of the function performs the NSB setup and this takes some time (about a second). However, subsequent executions have all the NServiceBus setup performed already and execute very rapidly. 

Building out NServiceBus endpoints on Azure Functions seems like a great way to get the best of both worlds. It isn't necessarily the best option for hosting every endpoint but for endpoints which are either very infrequently called or endpoints which are required to scale up and down rapidly it seems like a great approach. I'm also excited by some of the possibilities integrating more tightly with existing Azure services the bindings for which are in Functions already. The SignalR service in particular helps fulfill my dream of entirely serverless applications which can push out real-time updates. 
