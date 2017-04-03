---
layout: post
title: Content Security Policy
authorId: simon_timms
date: 2013-01-18
---

A few weeks ago I was doing some research into web application security to placate some security concerns a security audit raised. For the most part what I found was the typical advice

- <span style="line-height:14px;">Avoid SQL injection attacks by using [parameterized queries](http://blogs.technet.com/b/neilcar/archive/2008/05/21/sql-injection-mitigation-using-parameterized-queries.aspx)</span>
- <span style="line-height:14px;">Use low privilege accounts to run the web server and the database</span>
- Don't connect to the SQL server with an account with permissions other than db_reader and db_writer(or your database'sequivalent)
- Validate user input
- HTML encode any untrusted string([so pretty much everything](https://www.owasp.org/index.php/XSS_Filter_Evasion_Cheat_Sheet))
- Avoid using [dynamic SQL](http://taylorza.blogspot.ca/2009/04/sql-injection-are-parameterized-queries.html)

Onerelativelynew development I hadn't heard of was using a technique called Content Security Policy. To understand the purpose of CSP you first need to know a little bit about what comes down the line to you from the web server and how the browser handles it.

For many years, until Chrome came a long a messed it all up, the first 7 characters of every URL you saw was http://. This was a protocol identifier just like ftp:// or smb://. Chrome dropped showing it, something I agree withentirely. The protocol HTTP is the language which web servers speak. One of the things which HTTP defines are things called headers. These headers provide meta-information related to the request or the response. For the most part you can be a web developer and never look at HTTP headers. It is the headers which contain POST parameters as well as Accept-Types and Content-Types. The body of the HTTP response contains the HTML, CSS and JavaScript which define the page. Within that markup you can define external resources to load be they images, scripts or style sheets.

CSP is another header which describes the behaviour of the browser when it comes to loading the external resources and processing internal scripts. There is a wide variety of things which can be done using CSP but the most useful is to block the execution of inline JavaScript.

What?

How is my JavaScript going to get run if I can't have it inlined? Well pretty simply you make all of your javascript included from external files.

Why?

Because one of the most common attacks against sites it to inject some nefarious JavaScript in a way that it is rendered out when other users are logged in. By doing so you can grap their cookie information or information on the page. Think about a site with a comment system, if a bad guy injects some javascript code which can run in the context of other users then they can perform any action that the logged in user can. If all your JavaScript comes from static JavaScript files then there is no attack vector to exploit.

To get this working you're going to need to add 3 different headers to your site. This is because the various browsers have differing levels of support for CSP.

```
Content-Security-Policy: script-src 'self' 
X-WebKit-CSP: script-src 'self'
X-Content-Security-Policy: script-src 'self'
```

The first line is the policy as defined by the [W3 standard](http://www.w3.org/TR/CSP/), it is supported only by chrome and even then only by version 25+. The second version works in older WebKit based browsers. The third is supported by FireFox and IE10+. The support for CSP across browsers is not fantastic. At the time of writing there is support for about [55% of users](http://caniuse.com/contentsecuritypolicy).

There are quite a few rules you can use in the CSP. The rules above require that all your scripts come from your domain. If you make use of a CDN then you can add it to the end of the rules

```
Content-Security-Policy: script-src 'self' https://youcdn.com
X-WebKit-CSP: script-src 'self' https://yourcdn.com
X-Content-Security-Policy: script-src 'self' https://yourcdn.com```

You can also set the default so that all resources (scripts, style, images, flash, frames, fonts) are restricted to your server.

```
Content-Security-Policy: default-src 'self' https://youcdn.com
X-WebKit-CSP: default-src 'self' https://yourcdn.com
X-Content-Security-Policy: default-src 'self' https://yourcdn.com```

Once you get your head around not being able to use inline JavaScript then CSP is a clear win and should probably be the default when you create a new project.



