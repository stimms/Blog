---
layout: post
title: Surrogate Keys are King
authorId: simon_timms
date: 2013-05-27
---

Choosing keys in a database can be a pretty difficult process. In university it was very simple, we came up with superkeys and compound keys and made sure things were in BCNF. In the real world that doesn't work. Dealing with normalized databases is a huge pain. The other thing which is painful is dealing with compound keys where a record is referenced by two or more fields. I always recommend adding a unique identifier for each row of data. That key can be either an integer or a GUID, I know of arguments both ways and that isn't what this post is about.

Iwas recently sitting in on the evaluation of a piece of software. It was the standard CRUD sort of application with a few additional features. However one of the things which was permitted was allowing users to set the value of the primary key field. As soon as the presenter mentioned it hand shot up all over the room.

"What if we have the same object ID for multiple things?" "Err, well that would never happen." "Not true, we have 8 examples of it right here off the top of our head" "Err, well I guess we don't support that situation"

We talked it around for half an hour and the consensus was that this was a serious design flaw in the application. The developers had assumed that a certain piece of user generated data was certain to be unique but, as it turns out, it wasn't. Things got worse the further we pushed into the problem. There were other restrictions which further limited users' ability to enter the data they wanted in the primary key field.Unfortunately,the work needed to change the constraints was considerable as the primary key was, as primary keys are, heavily used throughout the application.

As a general rule I never allow users to enter key information because they may make mistakes. Mistakes are made all over the place by users but these tend to have more direconsequencesas they cross from user data over into system data. Think about what would happen if the user wants to change the value they entered in that field. You now need to crawl through countless other tables and data structures looking for places where that key was used and change it.

I consider there to be a division in the application between data used by the system and data used by users. You wouldn't want users seeing your application's routing tables because it isn't an abstraction about which they should have to care. Equally the IDs of records should not be visible to users. Just as you keep this data away from users you should keep the user data away from the application. I can't count the number of problems I've seen which, when you drill down, turn out to be a result of the system relying on user data which becomes inconsistent.





