---
layout: post
title: The Big Problem in Development
authorId: simon_timms
date: 2013-06-21
---

I frequently hear people saying "the difficult problem in computing is X" or "nothing is hard in computing but Y" for some value of X and Y. The most common one I hear is [Phil Karlton's quote](http://martinfowler.com/bliki/TwoHardThings.html)

There are only two hard things in Computer Science: cache invalidation and naming things.

I think that both naming and cache invalidation are hard but I don't think they are the hardest problems in computing science. Nor do I think "Is P == NP?" is the hard problem. It is certainly hard but it isn't all that important except for a certain class of problems. Maybe I'm doing the wrong things with my life but I can count on two hands the number of times I've run into a problem where the solution was a linear approximation or asearchin NP space.

No I think the big problem is: how is this going to change in the future?

Every design decision is informed by how much you're going to need to change the code infuture and in what ways it is going to change. Should you use dependency injection? The answer lies in whether you expect new implementations to be needed in future. Should you use a message based architecture? Well are other services going to be interested inreceivingnotifications in future? How about deploying your reports to the client or to the server? If the reports are going to change frequently then to the server but if they're static then client deployment provides a better user experience.Even problems like designing for scalability are informed by a knowledge of just how much the application is going to need to scale in the future.

The worst part about this problem is that it is clearly unsolvable. All you can do is guess about how maintainable and future proof your code base needs to be and make a decision. No matter should you be toopessimisticor too positive about your guess there is going to be a cost for guessing wrong. I'm a software guy more than a business guy so I think that the best bet is to guesspessimisticand build your application to be moreresilientto change. Business people might side with building the application as quickly and easily as possible because you don't know if it is going to succeed in the first place.

Make no mistake, it is aconundrum. When you pay for senior developers and architects you're paying for people who've been around enough to make good guesses. In my experience good guessing more than pays for the cost of hiring good developers.

If you happen to come up with a way to tell the future and solve this problem then let me know. You could just let me know by sending me the winning lotto numbers for the next 5 or 6 major lottery jackpots.



