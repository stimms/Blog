---
layout: post
title: Building a github page
authorId: simon_timms
date: 2013-06-10
---

I hear that the way github as a company works is that if you work there and think that github needs a feature you just build it. One of the features I've just recently discovered are github pages. Pages allow projects and people to put up simple static pages to communicate about the project.

I happen to do some development on a little project called [Angela Smith](http://blog.simontimms.com/2013/02/25/using-realistic-data-in-unit-testing/ "Using Realistic Data in UnitTesting")and it needs a website. At the moment all the documentation is scatted over a series of blog posts by the various contributors. It would be great to bring that all together.

It was pretty simple to set up the most basic of pages. I checked out the latest version of AngelaSmith and created a new branch.

git branch gh-pages git checkout gh-pages

The gh-pages branch is a special branch which is monitored by a build process at github. When it detects a new checkin it will build the site and deploy it to <username>.github.io/<project name>. In my case that meanshttp://stimms.github.io/AngelaSmith/.

That's not a very friendly name so it is possible to set up a custom url to point at that.

You can also use the Jekyll static page generator to build pages from templates and using includes. There is a bunch of documentation out there on how to use Jekyll. I have to say that while the documentation looks good it isn't super informative. It took me quite some time to figure out that the character encoding was very important. I ended up backing my content down to code page 437 and not UTF-8.

Once I get Jekyll figured out I'll put up more posts on it.



