---
layout: post
title: Select your JSON
authorId: simon_timms
date: 2013-03-29
---

In yesterday's post I talked about how to change out the JSON serializer in ASP.net MVC. That was the first step to serializing an NHibernate model out to JSON. The next issue I camacrosswas that the serialization was really slow. What's the deal with that? I looked at the JSON which was produced by the serialization and found that a single record was 13KiB! Even a modest result from this action was over 250KiB. This is a pretty significant amount of data to serialize. There was really no need to serialize that much data. In the end the data was being used to populate a couple of columns in a table.

The reason the data was so huge is that the NHibernate object which was being serialized was fully populated with data fromseveraltables. There was even a collection or two of records in there.

Projections to the rescue!

The beauty of returning JSON is that it isn't strongly typed. This means that all you need to do to trim down the traffic is to use object builder notation to project from the collection into a new object. To do this you need only do something like

results.Select(x=>new{ID=x.ID,Client=x.Client.Name,UserName =x.User.Name,Name=x.Name,Date=x.Date})

In my case this reduced the JSON payload from over 250KiB to 2.5KiB. This reduced not only the serialization time but also the time taken to send the traffic over the network.

This is a really easy optimization and it is something that tends to slip the mind. This is why it's important to test with large dataset.



