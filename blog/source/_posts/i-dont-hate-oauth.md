---
layout: post
title: I don't hate OAuth
authorId: simon_timms
date: 2013-06-20
---

It's weird that I don't hate OAuth. It is a combination of lots of things I hate: A complicated protocol and supported by Facebook(who I strongly dislike). Yet OAuth and OpenID are both technologies I support fully.

OpenID is a method of delegating authentication to a third party. So say I wanted to have user accounts on my site but I didn't want to go through the trouble of hashing passwords (you're not hashing with SHA are you?). Instead I can delegate all themessinessof storing password information with a third party. When a user signs in to my site I'llactuallyhave them sign in with their choice of account. This third party will pass a token back to my site to let me know that the user did sign in successfully. Any time you see those sign in with buttons chances are that it is implemented using OpenID.

[![OpenID Login](http://stimms.files.wordpress.com/2013/06/screen-shot-2013-06-19-at-9-34-36-pm.jpg?w=750)](http://stimms.files.wordpress.com/2013/06/screen-shot-2013-06-19-at-9-34-36-pm.jpg)OAuth is a similar concept to OpenID except that instead of the third party site giving me an authentication token I ask it for permission to access a protected resource. So if I waswritinga tool for displaying tweets in your timeline I would need to access the protected information held by twitter. My application would refer you to the server(Twitter) which would ask for your password and then refer the session back to my application. My application never sees your password, instead if my app is accessing Twitter you can remain confident that only Twitter is getting your password.

I really like the idea that only tokens are passed around and never passwords. Being able to revoke the access of applications to a protected resource at any time without invalidating your password.

OAuth has a reputation for being not just difficult to implement but also inconsistent from one implementation to another. This is notwhollyan undeserved reputation. The fact that there exist two competing standards 1.0a and 2.0 doesn't help at all. There is some argument that 2.0 is less secure and probably[ should not be used](http://hueniverse.com/2012/07/oauth-2-0-and-the-road-to-hell/). I'm not versed enough to give anopinionon that.

If you want to provide API access to your data then OAuth is probably worth looking into even ifimplementationsare a bit spotty.



