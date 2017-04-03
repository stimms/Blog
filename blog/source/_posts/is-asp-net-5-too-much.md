---
layout: post
title: Is ASP.net 5 too much?
authorId: simon_timms
date: 2014-11-11
---

I've been pretty busy as of late on a number of projects and so I've not been paying as much attention as I'd like to the development of ASP.net vNext, or as it is now called, ASP.net 5. If you haven't been watching the development I can tell you it is a very impressive operation. I watched two days worth of presentations on it at the MVP Summit and pretty much every part of ASP.net 5 is brand new.

The project has adopted a lot of ideas from the OWIN project to specify a more general interface to serving web pages built in .net technologies. They've also pulled in a huge number of ideas from the node community. Build tools such as grunt and gulp have been integrated into Visual Studio 2015. At the same time the need for Visual Studio has been deprecated. Coupled with the open sourcing of the .net framework developing .net applications on OSX or Linux is perfectly possible.

I don't think it is any secret that the vision of people likes Scott Hanselman is that ASP.net will be a small 8 or 10 meg download that fits in with the culturebeing taught at coding schools. Frankly this is needed because those schools put a lot of stress on platforms like Ruby, Python or node. They're pumping out developers at an alarming rate. Dropping the expense of Visual Studio makes the teaching of .net awhole lot more realistic.

ASP.net 5 is moving the platform away from propriatary technologies to open source tools and technologies. If you thought it was revolutionary when jQuery was included in Visual Studio out of the box you ain't seen nothing yet.

The thought around the summit was that with node mired in the whole [Node Forward](http://nodeforward.org/) controversy there was a great opportunity for a platform with real enterprise support like ASP.net to gain big market share.

Basically ASP.net 5 is ASP.net with everything done right. Roslyn is great, the project file structure is clean and clear and even packaging, the bane of our existence, is vastly improved.

But are we moving too fast?

For the average ASP.net developer we're introducing at least

- node
- npm
- grunt
- bower
- sass/less
- json project files
- dependency injection as a first class citizen
- different directory structure
- fragmented .net framework

That's a lot of newish stuff to learn. If you're a polyglot developer then you're probably already familiar with many of these things through working in other languages. The average, monolingual, developer is going to have a lot of trouble with this.

Folks I've talked to at Microsoft have likened this change to the migration from classic ASP to ASP.net and from WebForms to MVC. I think it is a bigger change than either of those. With each of these transitions there were really only one or two things to learn. Classic ASP to ASP.net brough a new language on the server (C# or VB.net) and the integration of WebForms. Honestly, though, you could still pretty much write ASP classic in ASP.net without too much change. MVC was a bit of a transition too but you could still write using Response and all the other things with which you had built up comfort in WebForms.

ASP.net 5 is a whole lot of moving parts build on a raft of technologies. To use a Hanselman term is is a lot of lego bricks. A lot of lego can be either make a great model or it can make a mess.

[![OLYMPUS DIGITAL CAMERA](https://stimms.files.wordpress.com/2014/11/legomess.jpg)](https://stimms.files.wordpress.com/2014/11/legomess.jpg)I really feel like we're heading for a mess in most cases.

ASP.net 5 is great for the expert developers but we're not all expert developers. In fact the vast majority of developersare just average.

So what can be done to bring the power of ASP.net 5 to the masses and still save them from the mess?

1. Tooling. I've seen some sneak peeks at where the tooling is going and the team is great. The WebEssentials team is hard at work fleshing out helper tools for integration into Visual Studio.

2. Training. I run a .net group in Calgary and I can tell you that I'm already planning hackatons on ASP.net 5 for the summer of 2015. It sure would be great if Microsoft could throw me a couple hundred bucks to buy pizza and the such. We provide a lot of training and discussion opportunity and Microsoft does fund us but this is a once in a decade sort of thing.

3. Document everything, like crazy. There is limited budget inside Microsoft to do technical writing. You can see this in the general decline in the quality of documentation as of late. Everybody is on a budget but good documentation was really made .net accessible in the first place. Documentation isn't just a cost center it drives adoption of your technology. Do it.

4. Target the node people. If ASP.net can successfully pull developers from node projects onto existing ASP.net teams then they'll bring with them all sorts of knowledge about npm and other chunks of the tool chain. Having just one person on the team with that experience will be a boon.

The success of ASP.net 5 is dependent on how quickly average developers can be brought up to speed. In a world where a discussion of dependency injection gets blank stares I'm, frankly, worried. Many of the developers with whom I talk are pigeon holed into a single language or technology. They will need to become polyglots. It is going to be a heck of a ride. Now, if you'll excuse me, I have to go learn about something called "gulp".



