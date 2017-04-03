---
layout: post
title: Quick A/B Testing in ASP.net MVC - Part 1 Using 2 Pages
authorId: simon_timms
date: 2014-01-06
---

A/B testing is a great tool for having your users test your site. It is used by some of the largest sites on the internet to optimize their pages to get people to do exactly what they want. Usually what they want is to drive higher sales or improve click-through rates. The key is in showing users two versions of a page on your site and then monitoring the outcome of that page view. With enough traffic you should be able to derive which page is doing better. It seems somewhat intimidating to run two versions of your website and monitor what people are doing, but it isn't. First it isn't your entire site which is participating in a single giant A/B test, rather it is a small portion of it â€“ really the smallest portion possible. In ASP.net MVC terms it is just a single view.

This blog is the first in a series about how to implement A/B testing easily in ASP.net MVC by using Redis

1. Having 2 pages
2. Choosing your groups
3. Storing user clicks
4. Seeing your data

The first thing we need is a website on which to try this out. Let's just use the default ASP.net MVC website. Next we will need two versions of a page. I'm going to pick the about page which looks like

[![about1](http://stimms.files.wordpress.com/2014/01/about1.png)](http://stimms.files.wordpress.com/2014/01/about1.png)Next we need to decide what we're trying to get people to do on the about page. I think I'm most interested in getting them to go look at the source code for the website. After all we like open source and maybe if we get enough people on our github site they'll start to contribute back and grow the community. So let's build an A version of the page.

[![about2](http://stimms.files.wordpress.com/2014/01/about2.png)](http://stimms.files.wordpress.com/2014/01/about2.png)This is a very simple version of the about page, good enough for a version 1.0. But we really want to drive people to contribute to the project so let's set up another page.

[![about3](http://stimms.files.wordpress.com/2014/01/about3.png?w=750)](http://stimms.files.wordpress.com/2014/01/about3.png)Nothing drives contributions like a giant octo-cat. To do this I set up two pages in my views folder, aboutA.cshtml and aboutB.cshtml

[![about4](http://stimms.files.wordpress.com/2014/01/about4.jpg)](http://stimms.files.wordpress.com/2014/01/about4.jpg)The first contains the markup from the first page and the second the markup for the second page. Now we need a way to select which one of these pages we show. I'd like to make this as transparent as possible for developers on the team. The controller code for the view currently looks like

<script src='https://gist.github.com/stimms/8277141.js'></script>

This is, in fact, the stock page code for ASP.net MVC 5. I'm going to have this page, instead, extend a new base class called BaseController which, in turn, extends Controller. Pretty much every one of my ASP.net MVC applications follows this pattern of having my controllers extend a project specific base class. I'm generally not a huge fan of inheritance but in this case it is the easiest solution. Altering the base class from Controller to BaseController is all that is needed to activate A/B testing at the controller level.

In the base class I have overridden one of the View() signatures.

<script src='https://gist.github.com/stimms/8277212.js'></script>

In the override I intercept the view name and, if the original view name doesn't exist I test for A and B versions of the view.

<script src='https://gist.github.com/stimms/8277249.js'></script>

Really the only trick here is to note how we find the viewName if it doesn't exist: we take it from the action name on the route. Checking the source for ASP.net MVC(helpfully open sourced, thanks Microsoft!) we find that this is exactly approach they take so behaviour should not change. You'll also notice that we call out to an ABSelector to chose which view to show. I'll cover some approaches to building the ABSelector in part II of this series. For now the code is available on github at[https://github.com/stimms/RedisAB](https://github.com/stimms/RedisAB).

I hope to have part II out in the next week.



