---
layout: post
title: Gotcha for AngularJS for Windows 8 JavaScript
authorId: simon_timms
date: 2013-06-04
---

I've been playing around a bit with angularJS and Windows 8 JavaScript. For the most part it has been a pleasant sort of endeavour. I did run into one thing for which I had to find a solution and I thought I would post it here.

The JavaScript context of WinJS applications is slightly more restrictive in some ways than IE10 JavaScript on which it is based. One of the limits is around adding content to a page. Because the Windows 8 apps have more power over the local file system than the sandboxed IE10 implementation there are some increased risks to allowing arbitrary content to be added to the page.

The functions

- innerHTML
- outerHTML
- document.write

Are all illegal functions. Instead one should be making use of

- innerText
- document.createElement
- setAttribute

There are two posts on the topic

- [http://onehungrymind.com/windows-8-and-angularjs/](http://onehungrymind.com/windows-8-and-angularjs/)
- [http://blog.jonathanchannon.com/2013/01/24/using-angularjsbackbonejs-in-windows-8-javascript-app/](http://blog.jonathanchannon.com/2013/01/24/using-angularjsbackbonejs-in-windows-8-javascript-app/)

However both of them are out of date as they reference custom builds of jQuery. As of jQuery 2.0 you don't need to use these tricks to get jQuery to work. However I did find that the latest angular still had an issue. In the final line of angular some styling information is injected onto the page.

angular.element(document).find('head').append('<style type="text/css">@charset "UTF-8";[ng\:cloak],[ng-cloak],[data-ng-cloak],[x-ng-cloak],.ng-cloak,.x-ng-cloak{display:none;}ng\:form{display:block;}</style>');

As you can see this just uses an append. Now append typically just calls appendChild which is permitted by WinJS. I still found it to be an issue, perhaps because we're appending a child with properties instead of a child with properties set with setAttribute. I solved it by using the [execUnsafeLocalFunction ](http://msdn.microsoft.com/en-us/library/windows/apps/Hh767331.aspx)method in WinJS. This overrides the security restrictions for the function passed in as an argument. I was confident the angular code was not going do harm so I felt okay about using this. In general I would take the time to rewrite the code to be more secure.

MSApp.execUnsafeLocalFunction(function () { angular.element(document).find('head').append('<style type="text/css">@charset "UTF-8";[ng\:cloak],[ng-cloak],[data-ng-cloak],[x-ng-cloak],.ng-cloak,.x-ng-cloak{display:none;}ng\:form{display:block;}</style>'); }); You just need to make sure to include angular after the Microsoft JavaScript libraries.



