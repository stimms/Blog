---
layout: post
title: OData
authorId: simon_timms
date: 2013-05-15
---

This is another brain dump of notes fromPrairieDev Con. this talk is Mark Stafford's OData talk

- <span style="line-height:13px;">REST isinsufficientlydescriptive for many applications</span>
- There a number of different options to create OData, even 2 on the Microsoft stack. WCF data services or ASP.net MVC WebAPI.
- OData supports more complicated queries than REST by itself. While REST can be used to do more complicated queries it is not designed to do so. As a result there is a lack of consistency in how people implement it.
- The WebAPI method is based on an MVC project. It basically just exposes an IQueryable to the web
- To build the controller you can extend from the EntitySetController<EntityType, EntityKeyType>. From there you just need to override the various methods

<script src='https://gist.github.com/stimms/5534015.js'></script>

- You still need to specify the individual end points
- You can specify the format returned using either the URL or using accept headers
- While the underlying OData library is mature WebAPI isn't. If there is something which is not implemented yet then you can implement a method called HandleUnmappedRequest
- There is support for retrieving data directly into Excel
- There is some really snazzy ability to do geospacial queries
- There is no real need to have your OData end points map directly to entities in the data model. They can easily map to projections so long as you can provide an IQueryable wrapper for the projection

Thoughts:

Yeah, REST is not great for doing querying. The excel retrieval is brilliant. Also being able to easily dig into queries just using a browser is great. I have some concerns that allowing querying against the database like this opens up the possibility of allowing users to run super inefficient queries and bring your system down. Users can't be expected to know what queries are going to do that so some sort of throttling or caching would be useful.



