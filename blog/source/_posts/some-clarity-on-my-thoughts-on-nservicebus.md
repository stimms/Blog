---
layout: post
title: Some Clarity on my Thoughts on NServiceBus
authorId: simon_timms
date: 2017-03-25
---
In my last blog post I mentioned in passing something about NServiceBus

```
There is, of course, a cost to running NServiceBus as it is a commercial product. 
In my mind the cost of NServiceBus is well worth it for small and medium 
installations. For large installations, I'd recommend building more tightly on 
top of cloud based transports, but that's a topic for another blog post.
```

I though that, perhaps, there should be some clarity to my comments. I have a few good friends who make their living working with NServiceBus and there was some debate about my point. 

<!-- more -->

MassTransit is a competitor to NServiceBus and I'd also say that Akka.NET plays in a similar enough space to be considered competition. However, one of the biggest competitors is hubris. The belief, by developers, that they can build NServiceBus in an afternoon. Afterall it is just a sprinkling of dependency injection on top of a transport protocol and mixed with a sprinkle of quartz.net, right?

I fell into that trap some years ago. The company I was with needed a service bus but wasn't willing to spend the money on NServiceBus. They had almost a dozen servers handling messages so the cost would have been pretty significant. I took a full month to build what was a prety limited version of NServicebus. It was an adventure and the result was, almost certainly, full of bugs. Hundreds of unit tests can uncover and verify a lot but when it comes to distirbuted, multi-processor systems the bugs tend to be, well, interesting. Building NServiceBus is jolly hard. 

This is the old build vs. buy dilema rearing it's head. The cost of building and maintaining a system the equivilent to NServieBus is huge. You would need to have a large team dedicated to it. However you don't necessarily need to build the entirety of NServieBus. That's the crux of my comment: in large systems you may wish to build your own, thinner, interface over a messaging system like Azure Service Bus: one which exposes a smaller, more focused API. Balancing build and buy is one of the more difficult things to get right in software. 

Consider a system like LMAX: it was a highly focused, message based architecture. It would not have been suitable for NServiceBus because they had performance needs which were well outside of NServiceBus' capabilities. Specific needs are another reason you might want to build your own system. 

I suspect that more than 95% of distributed systems written in .NET are suitable for simply using NServiceBus. It is that final few percent where the needs are unusual or specific enough to require a custom build.  

So should you buy NServiceBus? If your needs are for a reliable distributed system with top tier support then probably. If you have a massive system to build or narrow needs then you may wish to explore further. But, be warned, building distributed frameworks is exceedingly non-trivial. 
