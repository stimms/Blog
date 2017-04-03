---
layout: post
title: AngularJS
authorId: simon_timms
date: 2013-05-14
---

[![AngularJS](http://stimms.files.wordpress.com/2013/05/angularjs.jpg)](http://stimms.files.wordpress.com/2013/05/angularjs.jpg)This is another brain dump of content from PrDC13. This is [David Mosher](https://twitter.com/dmosher)"˜sexcellentAngular JS talk. Don't worry it is late on Tuesday when I'm writing this and the conference is almost over. Only a couple more days and we'll be back to more sensible posts. Likely I'll base some of my upcoming posts on stuff from this conference on which I would like to expand.

- AngularJS is a framework for building applications on the web. They don't have to be single page but you can certainly use id
- When creating an angular application you start by annotating the html element with ng-app="someappname"
- the angualr configuration works by doing angular.module("someappname", []);
- Angular templates are just html files
- You can put in the ng-view attribute on a div which is the container into
- Templates are cached automatically
- The module is set up and looks like

<script src='https://gist.github.com/stimms/5534843.js'></script>/4daa347c7108c76faf51e287716954b1bf94be8b

- <span style="line-height:13px;">Model binding is super simple you just need to annotate fields with ng-model="somemodel.somefield"</span>
- Angular has an internal loop which synchronizes the model with the view. If you make changes outside of the normal way then you need to call $scope.$apply()

<script src='https://gist.github.com/stimms/5534843.js'></script>/33f9138d6e1d3d0fea466068eb199179c1765cdf

- Functionality can be extracted into services
- <span style="line-height:13px;">There is an issue with minification which can break the dependency injection for Angular. You can either change your functions to be an array listing the dependencies or use [ngmin](https://github.com/btford/ngmin)to preprocess your code</span>



