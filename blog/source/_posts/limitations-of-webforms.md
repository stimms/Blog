---
layout: post
title: Limitations of WebForms
authorId: simon_timms
date: 2014-04-18
---

I'm spending a lot of time working with WebForms at the moment. I haven't written WebForms forms since"¦ 2003 maybe 2004. When WebForms was created it was done as way to transition developers from the drag and drop world of Windows development to the exciting world of the Internet. Of course the Internet is not a Windows form. The result is that WebForms is a leaky abstraction. The abstraction has been getting leakier and leakier as web technology progressed.

One of the key features of WebForms is that it keeps track of transient data in ViewState. By using ViewState WebForms is able to provide a web experience which is more similar to a desktop application: it is expected that when pressing a button on a Windows form that the rest of the form isn't wiped out. This would happen without a ViewState storing the form state.Depending on how you configure your application the ViewState is either kept in a hidden field which is sent to the client or in some sort of server side storage mechanism. You can hook ViewState persistence up to a database like SQL server or to a distributed cache like memcache or Azure cache.

However the vast majority of sites keep ViewState in that hidden field. As your ViewState grows then so does each page load. There is very little room to optimize ViewState because, instead of it being sensibly stored in a key value fashion it is persisted as a single blob. The entire thing is persisted and reloaded each time. The result is that most interactions with the server need to include this viewstate. This makes lightweight AJAX calls difficult. When AJAX started to become popular update panels were introduced. These were chunks of the page which could be refreshed independently.

Again these were justplugging up a leaky abstraction.

As web applications become more JavaScript based it became apparent that the HTML produced by WebForms was brittle. Controls were named with a near indecipherable id which changed based on the rest of the page. Later versions of ASP.net brought more predictible contol names but, again, this was just patching the abstraction. If you're interested in building a modern web application it should not be done using webforms. There are just too many places where the abstraction leaks and makes your job much more difficult.

Modern web applications make much greater use of client side framework the likes of Angular, Ember and Backbone. With this class of application the server side framework starts to matter less and less. Eventually it is reduced to a tool for sending views to the client and providing data end points. I won't miss WebForms on new applications but we're not all lucky enough to work on new applications. For legacy applications which are written using WebForms there are upgrade paths available to you.

I'm going to start blogging a bit over the next few weeks about how to start taking steps towards more modern WebForms applications without jeopardizing existing functionality. Stay tuned!



