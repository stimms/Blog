---
layout: post
title: Glimpse for raw ADO
authorId: simon_timms
date: 2014-04-21
---

If you haven't used [Glimpse ](http://getglimpse.com)and you're developing a web application in .net then you're missing out on a great deal of fun. Glimpse is sort of like the F12 developer tools but running on your server and offering insight into how the server side of the page is doing. Installing Glimpse is also very easy and can be done entirely from nuget.

install-package Glimpse.AspNet

Because there are all sorts of different configurations for ASP.net applications Glimpse is divided up into a main assembly and then a bunch of helper assemblies which can be put together like Lego. For instance if you're using a brand new ASP.net MVC site with Entity Framework then you can install Glimpse and then the modules for that specific configuration

install-package Glimpse.EF6 install-package Glimpse.MVC5

This is a very well supported configuration. If you're less fortunate and you need to hook Glimpse into an application which makes use of a lot of legacy data layer code then EF profiling isn't going to be available for you. You can, however, make use of the ADO Glimpse package. To do this you'll first need to install the package

install-package Glimpse.ADO

If you're application makes use of DbProviderFactory then you're done. If not then you'll need to transition the site to make use of this method of building connections.DbProviderFactory is a method of abstracting out the connection provider so that you can easily swap different database strategies into place. If you were originally setting up connections to open like so

<script src='https://gist.github.com/stimms/11150447.js'></script>

Then all you need to do is convert it to look like

<script src='https://gist.github.com/stimms/11150505.js'></script>

The issue is that the ADO tooling for .net provides very few extension points. Glimpse.ADO works by hooking itself in as a DbProvider which wraps the SqlProvider and intercepts all the calls. You don't need to modify your entire site at once but if you don't you'll get a funny mixture of pages which work and pages which don't work. A few well crafted regular expressions got me 90% of the way there on a medium sized application and I did a full transition in about an hour so it isn't a huge time investment.

With the site using DbProviderFactory and Glimpseup and running I was able to get some really good hints about why pages were somewhat slow.

[![glimpse](http://stimms.files.wordpress.com/2014/04/glimpse.jpg)](http://stimms.files.wordpress.com/2014/04/glimpse.jpg)Having this sort of information exposed to developers makes it much easier to debug and solve performance issues before the site hits users.





