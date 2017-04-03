---
layout: post
title: Octokit.net - Quickstart
authorId: simon_timms
date: 2014-03-29
---

I'm working on a really nifty piece of code at the moment which interacts with a lot of external services and aggregate the data into a dashboard. One of the services with which I'm working is github. My specific need was, given a commit, what was the commit message?

GitHub have [a great RESTful API](http://developer.github.com/v3/) for just this sort of thing and they even have a .net wrapper library for the API called [Octokit.net](https://github.com/octokit/octokit.net). It seems to bind most of the API, which is great. It also seems to have no real documentation, which is not.

The repositories against which I wanted to fire the API were part of an organization and were private so I needed to authenticate. You have two options for authenticating against the API: basic or OAuth. As my service was going to be used by people who don't have github credentials the OAuth route was out. Instead I created a new user account and invited it into the organization. It is always smart to give as few permissions as possible to a user so I created a new team called API in the organization and made the API user its only member. The API team got only read permission and only to the one repository in which I was interested.

Next I dropped into my web project and added app settings for the user name and password. I use a great little tool called [T4AppSettings ](https://github.com/motowilliams/T4-AppSettings)which is available in nuget. It is a [T4](http://www.hanselman.com/blog/T4TextTemplateTransformationToolkitCodeGenerationBestKeptVisualStudioSecret.aspx)template which reads the configuration sections in your web.config or app.config and makes them into static strings so you don't need to worry about missing one in a renaming. The next step was to add a reference to Octokit

install-package octokit

in the package manager console did that. Then we new up some credentials based on our app settings

<script src='https://gist.github.com/stimms/9815797.js'></script>

Next create a connection

<script src='https://gist.github.com/stimms/9815842.js'></script>

The product header values seems to just be any value you want. I'm sure there is some reason behind it but who knows whatâ€¦ Now we need to get the octokit client based on this connection.

<script src='https://gist.github.com/stimms/9815882.js'></script>

That is all the boring stuff out of the way and you can start playing around with it. In my case I had a list of objects which contained the commit versions and I wanted to decorate them with the descriptions

<script src='https://gist.github.com/stimms/9815954.js'></script>

This was actually what took me the longest. The parameters to the Get were not well named so I wasn't sure what should go in them. Turns out the first one is the name of the owner where the owner is either the organization or the user. The second one is the name of the repository. So for this repository[![repo](http://stimms.files.wordpress.com/2014/03/repo.jpg)](http://stimms.files.wordpress.com/2014/03/repo.jpg)the owner is alexwolfe and the repository name is Buttons.

The GitHub API is rich and powerful. There is a ton to explore and many ways to get into trouble.Take chances,makemistakes,get messy.





