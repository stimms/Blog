---
layout: post
title: Content Security Policy for ASP.net MVC
authorId: simon_timms
date: 2013-01-21
---

In the[ last article](http://blog.simontimms.com/2013/01/19/content-security-policy/ "Content SecurityPolicy") we talked a bit about Content Security Policy. Now let's see how to quickly apply it to an ASP.net MVC project.

The ASP.net MVC project have provided some extension points in the lifecycle of a request which allow you to hook in almost as if you're using AOP. The one we'reinterestedin today is the global action filter. This is fired for every request and is an ideal place to put in a hook for adding HTTP headers.

First we create an action filter attribute which extendsActionFilterAttribute

<script src='https://gist.github.com/4579855.js'></script>

As you can see here I've put in all the different headers we talked about yesterday. You could make this more efficient by checking the browser and only sending the response which suits. That is kind of a pain to do as CSP is still in flux on most browsers. In a couple of years you will probably be able to only send one header.

Next we tie it into ASP.net MVC. You can throw it into the FilterConfig.cs file like so:

<script src='https://gist.github.com/4579907.js'></script>

(line 6 is the relevant one)

And you're done! I tested it by throwing in an inline alert(â€˜hi') and found it to be effective. Well effective in Chrome and FireFox. IE10 still merrily threw up an alert. IE10 support is not there yet, perhaps in IE11.

There is one other good way to add CSP to an ASP.net MVC project and we'll cover that in a future post.



