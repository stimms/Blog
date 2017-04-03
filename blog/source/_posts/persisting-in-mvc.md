---
layout: post
title: Persisting in MVC
authorId: simon_timms
date: 2009-11-14
---

Rob Conery, who is a giant in the ASP.net MVC world(he wrote the ASP.net store front and is also the author of a 200 page book on inversion of control) is [calling for suggestions](http://blog.wekeroad.com/subsonic/its-time-for-this-activerecordengine-for-asp-net-mvc/) about an active record persistence engine. I wanted to present how I think it should be done which is just a bit too long for a comment on the tail end of Rob's blog. I've been reading a lot lately about [areas in MVC2](http://haacked.com/archive/2008/11/04/areas-in-aspnetmvc.aspx) and the [portable areas project](http://www.lostechies.com/blogs/hex/archive/2009/11/01/asp-net-mvc-portable-areas-via-mvccontrib.aspx) which is part of MVC contrib project. Now the portable areas aren't yet finalized but the idea is that these areas will be able to be dropped into a project and will provide some set of controllers and views which will provide a swack of functionality.

[![](http://www.lostechies.com/cfs-file.ashx/__key/CommunityServer.Blogs.Components.WeblogFiles/hex/image_5F00_thumb_5F00_35686ED4.png)](http://www.lostechies.com/cfs-file.ashx/__key/CommunityServer.Blogs.Components.WeblogFiles/hex/image_5F00_thumb_5F00_35686ED4.png)

The example I've seen bandied about is that of a forum. Everybody has written a forum or two in their time now you can just drop in an area and get the functionality for free. I can see a lot of these components becoming available on codeplex or git hub. Component based software like this is "the future" just like SOA was the future a few years ago. The problem with components like this is that it is difficult to keep things consistent across the various components. At one end of the spectrum of self containment If each component is self contained then it has to provide for its own data persistence as well as any other services it consumes.

[![](http://stimms.files.wordpress.com/2009/11/paintdiagram.png?w=300)](http://stimms.files.wordpress.com/2009/11/paintdiagram.png)

I have helpfully harnessed the power of MS Paint to create an illustration of the spectrum between a component being self contained and being reliant on services being provided for it. If anybody is interested my artistic skills are available for hire. The further to the left the more portable the further to the right the more reliant the components are on services being provided for them and the less portable. We want to be towards the left, because left is right.

If these components really are the future then we need to find a way to couple the components and provide communication between them. This is where MEF steps up to the plate. What I'm proposing is that rather than spending our time creating unified interfaces for storing data we create a method agnostic object store. Components would call out to MEF for a persistence engine and then pass in whatever it was they wanted to save. The engine should handle the creation of database tables on the fly or files or web service callouts to a cloud. That is what I believe should exist instead of a more concrete IActiveRecordEngine.

What's the advantage? We keep the standard interface for which Rob is striving but we can now have that interface implemented by a plugable component rather than having it hard coded into a web.config.

The second part of Rob's post is about creating [opinionated controllers](http://flimflan.com/blog/SampleOpinionatedController.aspx). I have to say I'm dead against that. I agree with the goal of implementing the basic CRUD operations for people, in fact I'm in love with it. What I don't like is that it is implemented in a base class from which my controllers descend. If I'm reading the post correctly then the base controller is implementing actual actions. It is dangerous to implement actions willy nilly, actions which could be dangerous and people wouldn't even realize the actions exist. Chances are very good that users are just going to leave the actions implemented rather than overriding them with noop actions.

Another option is that I'm reading this post incorrectly and the methods in the base class are private and not actions. I like that a lot more, but even more I like generating template controllers. Subsonic 3 follows this methodology and it is really nice to be able to twiddle with bits of the implementation. What's more the generation doesn't have to stop at the controller. If the implementation in the controller is known then why not generate the views as well?

All in all I like the idea of improving the object store support in ASP.net MVC but I would like it to be as flexible as possible.



