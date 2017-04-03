---
layout: post
title: Document control and DDD/CQRS - solving similar problems
authorId: simon_timms
date: 2013-09-18
---

I had the good fortune to have a two hour introduction to the world of document control the other day. It was refreshing to see that we programmers aren't the only ones who don't have things figured out yet. The entire document control process is an exercise in managing the flow and ownership of data. I spent a lot of time thinking about how similar the document control problem and the data flow problem mirror each other.

Document control is really interested in documents and doesn't care at all about the contents of these documents. Their concerns are largely around

- who owns this document?
- what is the latest version of this document?
- how is this document identified?
- how long do I have to keep this document?
- is this document superseded by some other document?

These sound a lot like issues with which we deal when using DDD. Document ownership is a simply a problem of knowing in which aggregate root a document belongs. Document versioning is similar to maintaining an event stream. Document identification is typically done through numbering "“ however the flow of documents is slow enough that sequential numbering isn't a problem "“ no need for a randomly generated GUID.

Document retention isn't one with which we typically spend much time in CQRS land. Storage is cheap so we just keep every version around or at least we're able to generate every version through event sourcing. Perhaps the most congruent concept is taking snapshots of aggregates, but we're typically only interested in the most recent version of the aggregate. With document control there is always some degree of manual intervention with documents so there is a significant cost to retaining all documents indefinitely. I'm only talking about digital copies of document here, [Zuul ](http://ghostbusters.wikia.com/wiki/Zuul)protect you if you need to track paper copies of things too. I can't even keep track of my keys let alone tens of thousands of documents. My strategy for paper documents would be to burn them as soon as I got them and refer people to the digital version.

Superseding documents also doesn't seem like a problem we typically have in CQRS. In document control one or more documents may be supersededby one or more documents. For instance we may have a lot of temporary documents which are created by the business things like requests to move offices. They have value but only in a transitory way. Every week the new office seating chart is built from these office move documents and the documents discarded. Their purpose is complete and we no longer care about them as we have a summary document.

<div class="wp-caption aligncenter" id="attachment_2989" style="width: 311px">[![Many documents become one. I call it a Voltron operation. ](http://stimms.files.wordpress.com/2013/09/filterdown.jpg)](http://stimms.files.wordpress.com/2013/09/filterdown.jpg)Many documents become one. I call it a Voltron operation.

</div>In the opposite operation a document can be replaced by a series of documents. This activity is prevalent when adding detail to documents. A single data sheet may become several documents when examined in more detail

<div class="wp-caption aligncenter" id="attachment_2990" style="width: 311px">[![Reverse Voltron? Fan-out? The name may need some work. ](http://stimms.files.wordpress.com/2013/09/filterup.jpg)](http://stimms.files.wordpress.com/2013/09/filterup.jpg)Reverse Voltron? Fan-out? The name may need some work.

</div>This was originally going to be a post about how much we in the DDD/CQRS community have to learn from document control. I imagined that document control was a pretty old and well defined problem. There would surely be well defined solutions. I did not get that impression.

The problem of canonical source of truth or "who owns the data" is a very difficult one in document control. We're spoiled in DDD because it is rare indeed that the owner of a piece of data can change during its lifespan. Typically the data would remain within an AR and never updated without the involvement of the AR. With document control it is probable that responsibility could jump from your AR to some other, possibly unknown, AR. It could then jump back. At any point in time it would be impossible, without querying every AR, who had control of the data. Of course with a distributed system like many people working on a document it is possible that there will be disagreement about which AR has responsibility at any one time. Yikes!


# What we can learn from document control

I think that looking at document control gives us a window into what can happen when you relax some of the constraints around DDD. Data life-cycle is well defined in DDD and we know who owns data. If you don't then you end up in trouble with knowing who is the source of truth. Document control must solve this problem constantly and it can only be done by going out and asking stakeholders a lot of questions "“ a time consuming exercise.

The introduction of splitting and combining documents, or in our case aggregates, over their lifetime is disastrous. You lose out on the history of information and knowing where to apply events becomes difficult. Instead we should retain aggregates as unchanged as possible (in terms of what fields they have, obviously the data can change) and rely on projections of the data to create different views of information. This is basically impossible to apply to formatted documents as you would have in document control.


# What I think would help out document control

The first thing which comes to mind as being directly applicable to document control is removing meaning from the document identifiers. The documents document control manages tend to be numbered and the temptation to add meaning to a document number is too tempting to turn down. For instance you might get a number like

P334E-TT-6554

In our imaginary scheme all documents which start with P are piping diagrams. The 334 denotes the system to which it belong, E the operating pressure and TT the substance inside the pipe. The final digits are just incrementally assigned. The problem is just what you would expect: things change. When they do a decision must be made to either leave the number intact and damage its reliability or to renumber the document and lose the history. instead document control would do well to maintain an identifier whose sole purpose is to identify the document. The number can be retained but only as a field.

A more controversial assertion is that document control should retain all documentation. We retain a full history of messages used to build an entity, even if it is offline and used in favor of a snapshot. I believe that document control should do the same thing. Merging and splitting document is problematic and complicated. It is easier to just create a new document and reference the source documents. Ideally the generation of these new documents can be treated as a projection and the original documents retained.

In the end it is interesting to see how similar problem domains are solved by different people. That's the beauty in learning a new development language; every language has different features and practices. I'm not, however, prepared to be the guy who learns document control in depth to bring their knowledge back to the community.



