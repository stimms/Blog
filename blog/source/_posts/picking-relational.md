---
layout: post
title: Picking Relational
authorId: simon_timms
date: 2013-01-04
---

I'm really excited by the proliferation of databases which aren't relational in nature. There are almost too many variations to name now. The argument for making your data layer reallyplug-ableis much more compelling now. I was never convinced of the need previously because it seemed like such a zero-sum game to switch from SQL Server to Oracle to MySQL. The performance profiles were, for the most part, not all that divergent and any cost savings would probably be swallowed by retraining and updating the application. But now there can be significant differences in performance for certain tasks between SQL database and other types. This means that a decision which use to be easy, â€œWhich database should I use?â€, is vastly more complicated. As with all complicated decision in system design it is best to delay them until you have as much information as possible.

With all the choices available to developers for storing data I worry a bit that relational databases will be ignored in favour of other storage mechanisms. There is sill plenty of data which makes sense in a relational database. Heck, I would even claim that most of the data we deal with on a day to day basis makes the most sense in a relational database.

Even with these choices it seems that a good many shops remain coupled with SQL databases. I don'tentirelyblame them. If there is one technology which has been pretty slow to change it is databases. This is partly because for years we were complacent in thinking that SQL databases were the solution to all data problems andpartiallybecause there is so muchperceivedrisk in changing database technologies.

Therelationallyof most data coupled with the comfort that IT departments and developers have with relational SQL databases shouldn't be ignored when looking at data storage solutions. I would also consider that these databases have been around for many years and no matter what vendors like 10gen say they're likely to have more bugs than established SQL databases. Before you decide on using a newer database technology you need to be sure that there is a real need for it and you're not falling into the trap of doing whatever is new and cool. If you build in that data layer then swapping out the entire database or portions of the database is much easier.



