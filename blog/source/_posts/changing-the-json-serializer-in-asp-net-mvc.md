---
layout: post
title: Changing the JSON Serializer in ASP.net MVC
authorId: simon_timms
date: 2013-03-28
---

Today I stumbled onto some code which was serializing an NHibernate model to JSON and returning it to the client. The problem with directly serializing the object from NHibernate is that it may very well contain loops. This is fine in a C# object graph because only references are stored. However JSON doesn't have the ability to store references. So a serializer attempting to serialze a complex C# object must explore the whole object graph. The loop would make the object infinatly large.

The code in question returned a ContentResult instead of a JSON result. I didn't like that, why not just override the Json method? So I went to override the call and found, to my chagrin, that the base class, Controller, did not declare all the signatures as virtual. Bummer. I hate this sort of thing. Now serialization becomes something of which developers need to be aware. There are a couple of methods you can override but that's not a good solution as unless developer know about the custom serializer they won't know which methods they can an cannot call. I'm not sure what the motivation is around not providing a place to plug in a custom serializer but it does seem to me to be a pretty common extension point. The ASP.net MVC team are pretty good a what they do so I imagine there is a reason.

The best solution I could come up with in short order was to override the two signatures I could. To do that I wrote

https://gist.github.com/anonymous/5268866

I also created a NewtonSoftJsonResult which extended the normal JsonResult. This class provides the serialization implementation. In our case it looks like

https://gist.github.com/anonymous/5268878

I still don't like this solution but it does work. I'm going to think on this one.



