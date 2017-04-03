---
layout: post
title: Speeding up page loading - part 1
authorId: simon_timms
date: 2013-12-04
---

I started to take a look at a bug in my software last week which was basically â€œstuff is slowâ€. Digging more I found that the issue was that pages, especially the dashboard were loading very slowly. This is a difficult problem to definitively solve because page loading speed is somewhat subjective.

We don't have any specifications about how quickly a page needs to load on the site. Less than 5 seconds? Less than 2 seconds? Such a thing is difficult to define because all too often we fail to define for whom the page loading should be quick. Loading is governed by any number of factors

- time taken to build the HTML for the view (excuse the MVC style language â€“ the same token replacement needs to be done on most frameworks)
- speed of the server
- speed of the connection from the server to the client
- bandwidth between the client and the server
- speed of the client to render the HTML
- â€¦

The list is pretty daunting so I thought I would write about what I did to improve the speed of my application.

The application is a pretty standard ASP.net MVC application with minimal fron end scripting(at least compared with some applications). This means that the steps I take here are pretty much globally applicable to any ASP.net MVC website and many of them are applicable to any website. My strategy was to pick off the low hanging fruit first, fixing easy to fix problems and those which had a big impact on the speed. This would give some breathing room to get time to fix the harder problems.

This post became quite long so I've split it into a number of parts.

1. Bundling CSS/JS  
 2. [Removing images](http://blog.simontimms.com/2013/12/09/speeding-up-page-loading-part-2/ "Speeding up page loading â€“ part2")  
 3. Reducing Queries  
 4. Speeding Queries

I'll post the later parts as I finish writing them.


# Loading Resources

A web page is made up of a number of components each of which has to be retrieved from a server and delivered to a client. You start with the HTML and as the client parses the HTML it issues additional requests to retrieve resources such as pictures, CSS and scripts. There is a whole lot of optimization which can be done at this stage and it is where I started on this website.

I started on the slowest loading page: the dashboard. We're not live yet but it is embarrassing that on our rather limited testing data we're seeing page load times on the order of 15 seconds. It should never have got this far out of control. Performance is a feature and we should have been checking performance as we built the page. Never mind, we'll jump on this now.

My tool of choice for this is normally Google Chrome but I thought I might give IE11â€²s F12 tools a try. A lot of effort has been put in my Microsoft to improve Internet Explorer in the past few years and IE11 is really quite good. I have actually found myself in a position where I'm defending how good IE is now to other developers. I never imagined I would be in this position a couple of years ago, but I digress.

You can get access to the developer tools by hitting F12 and pressing the play button then reloading the page. This will result in something like this:

[![IE Profiling](http://stimms.files.wordpress.com/2013/11/screen-shot-2013-11-29-at-1-01-19-pm.jpg?w=750)](http://stimms.files.wordpress.com/2013/11/screen-shot-2013-11-29-at-1-01-19-pm.jpg)

This is actually the screen after some significant optimization. If you zoom in on this picture then you can see that this page is made up of 9 requests

- 1 HTML
- 3 CSS
- 2 Scripts
- 3 Fonts

Originally the page had several more script files and several images taking the total to something like 15. We want to attempt to minimize the number of files which make up a page as there is a pretty hefty overhead associated with setting up a new connection to the server. For each file type there is a strategy for reducing the number of requests.

HTML is pretty much mandatory. CSS files can be concatenated together to form a single file. Depending on how your CSS is constructed and how diligent you've been about avoiding reusing identifiers for different things across the site this step can be easy or painfully difficult. On this website I made use of the CSS bundling tools built into ASP.net. I believe that the templates for new ASP.net projects include bundling by default but if you're working on an existing project it can be added by creating the bundles like so

<script src='https://gist.github.com/stimms/7720263.js'></script>

You'll note that I'm registering the bundles twice, this is just to demonstrate that you can include either individual files or a whole directory. Then call out to this in the Global.asax.cs's application start

<script src='https://gist.github.com/stimms/7720195.js'></script>

You can now replace all your inclusions of CSS with a single request to ~/bundles/Style (don't worry about that ~/ razor will correctly interpret that for you and point it at the site root). If you look at the CSS file hosted there you'll see that it is a combined and whitespace-stripped file. This minimization will save you some bandwidth and is an added benefit to bundling.

JavaScript files can be bundled in much the same way. If you've been smart and namespaced your JavaScript into modules then combining JavaScript should be a sinch. Otherwise you might want to look into how to [structure your JavaScript](http://css-tricks.com/how-do-you-structure-javascript-the-module-pattern-edition/).Bundling the script files is much the same as the CSS

<script src='https://gist.github.com/stimms/7720310.js'></script>

The script bundle will concatenate all your script files together and also [minify](http://en.wikipedia.org/wiki/Minification_(programming)) them. This saves not only on bandwidth but also on the number of connections which need to be opened.

Reducing the number of requests is a pretty small improvement but it is also pretty simple to do. In the next part we'll look at removing images to speed up page loading.



