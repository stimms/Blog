---
layout: post
title: Building Web Sites with ASP.net MVC4
authorId: simon_timms
date: 2013-05-10
---

This is another brain dump from another session atPrairieDev Con. This session is by the reallyexcellentand hilarious [James Chambers](https://twitter.com/canadianjames).

- ASP.net MVC is built using the idea of convention over configuration.
- MVC is model view controller
- MVC and the tooling for it is now released out of band with Visual Studio
- MVC is built on top of ASP.net so the role providers from a WebFormsapplicationcan be plugged right in
- MVC is built on a bunch of nuget packages. If you have them already on your machine then there is no need to retrieve packages over the net
- There are a bunch of templates to choose from when you start a new asp.net mvc site - empty project is totally empty and almost always a bad idea
- basic at least adds some of the app_start stuff and is a good starting point
- Internet and intranet add authentication
- Authentication is now set up via the AuthConfig which is in App_Start. There is now out of the box support for OAuth
- Bundling reduces the number of requests to the server by smushing CSS and javascript together
- Routing allow for you to map urls to pages
- When creating a new controller it is possible to use scaffolding to create controllers which have some functionality built in
- The default built in data access stuff with the controller templates are built using EF
- You should refactor almost right away as the generated code has hard dependencies.
- The model binder, which can be overwritten, pulls information out of the request and binds it to a given model. It works on best effort so it is possible it won't be able to bind some fields. In that case it just skips the field.



