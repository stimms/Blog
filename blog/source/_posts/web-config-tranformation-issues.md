---
layout: post
title: Web.config Tranformation Issues
authorId: simon_timms
date: 2010-11-19
---

As you all should I am using [web.config transformations](http://msdn.microsoft.com/en-us/library/dd465326.aspx) during the packaging of my ASP.net web applications. Today I was working with a project which didn't have transformations defined previously so I thought I would go ahead and add them. All went well until I built a deployment package and noticed that my transformations were not being applied. Looking at the build log I found warnings like these

  
```
C:\Users\stimms\Documents\Visual Studio 2010\Projects\SockPuppet\Web.Debug.config(5,2): Warning : No element in the source document matches '/configuration'  
Not executing SetAttributes (transform line 18, 12)  
Output File: objDebugTransformWebConfigtransformedWeb.config
```

This started a fun bit of debugging during which I removed everything from the web.config and built it from the ground up. Eventually I traced the problem to a hold over from some previous version of .net, in the Web.config the configuration was defined as

```xml
<configuration xmlns="http://schemas.microsoft.com/.NetConfiguration/v2.0">
```

Changing that to just

```xml
<configuration>
```

Solved the problem. I imagine this is some sort of odd XML namespace issue, hopefully if you run into this you'll find this post and not waste an hour like me.



