---
layout: post
title: Why you shouldn't be bothering with routes
authorId: simon_timms
date: 2013-03-13
---

In my mind one of the most abused bits of functionality in ASP.net MVC is the routing engine. I don't think this is a problem unique to ASP.net MVC as other MVC frameworks like Rails also make heavy use of routing.

In a nutshell routing allows you to map the URL(or URI if you want to beentirelycorrect) to a file or action on the server. For instance if you look at the URL for this post

http://blog.simontimms.com/2013/03/04/why-you-shouldnt-be-bothering-with-routes

You can see that there is some information buit into it. I don't know the internals of wordpress but you can be pretty much assured that there isn't actually a file on disk calledwhy-you-shouldnt-be-bothering-with-routes in the directory/2013/03/04/. Instead this URL is used by a routing engine to passappropriateparameters to a script which looks up information in a database. The combination of the domain and the name of the blog is probably sufficient to identify the record in the database. The date information in there is just a hint to readers so they know when the blog was written.

The only thing is: its wrong. I'm writing this blog on the 9th of March and it is scheduled to be published on the 13th. The reason that date is there is that I created a draft on the 4th. That I created a draft on the 4th isimmaterial. I have some drafts from 9 months ago. Heck, I have a half written rant about an architectural failure at Backblaze which is so out of date that it will likely never be published. The draft date is not important in the least.Fortunatelynobody looks at the URL to gather this information.

URLs are not meant to convey useful information to people, they're there as instructions to computers. From time to time it is useful to have a friendly URL that people can remember but this is typically only for allowing them to type it in from a piece of paper. URLshorteningservices are fantastic for that.

Embedding information in the URL might look nice but it has very limited utility. In the .net world routing is provided by System.Web.Routing and this is typically configured in the startup of an MVC application. I have seen a number of MVC applications which have dozens, even hundreds of routes defined. Even Phil Haacked has contributed to the madness by providing a [route debugger](http://haacked.com/archive/2008/03/13/url-routing-debugger.aspx). Stop the madness! Use the default routes!

Having complex routes makes it very difficult to figure out which controller it is that is causing you trouble. It is also a boat load of extra code you need to test and understand. The fact that a route debugger is necessary should be a big hint that your logic is too complicated. The default route is sufficient for 99% of the requests coming to your site.

The only legitimate uses I can think of for routing requests are:

1. You need to do some optimization for search engines. Apparently google place some emphasis on the structure of a URL. It is difficult to tell because google keep pretty quiet about how they do page ranking. Optimizing for search engines is a prettysleazybusiness anyway so you're likely better of not even trying.

2. Mapping old URLs. This is a really good call. It sucks when there is a change to the underlying engine behind a website and now none of your links work. By setting up a mapping you can avoid a lot of 404s in your error logs.

If these two reasons don't match your use case then don't even bother adding custom routes. It will save you headaches in the future.





