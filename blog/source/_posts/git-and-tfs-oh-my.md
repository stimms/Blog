---
layout: post
title: Git and TFS, oh my
authorId: simon_timms
date: 2013-01-31
---

The huge news today was that the TFS team haveembracedgit to a crazy level. Theannouncementcan be read over at [Brian Harry's blog](http://blogs.msdn.com/b/bharry/archive/2013/01/30/git-init-vs.aspx). I haven't actually had a chance yet to use it but from what I can see git is a first class citizen in the TFS environment. Anything you could do with TFS source control you should be able to do now with git. This includes things like linking checkins to issues and sending git commits for code review.

At first glance that seems weird because what is TFS if it isn't source control. Well TFS is a lot more than source control. People have watched TFS grow out of Visual Source Safe and have assumed that it is just a continuation of the source control only tool which was VSS. TFS is actually a whole ecosystem of tools related to the lifecycle of software. There are [code review](http://tfs.visualstudio.com/en-us/learn/code/get-your-code-reviewed-vs/) tools, issue management tools, [backlog management](http://tfs.visualstudio.com/en-us/learn/collaborate/manage-your-backlog/) tools. There is even a really sweet[ stakeholder feedback system](http://tfs.visualstudio.com/en-us/learn/collaborate/feedback/) which is aphenomenaltool that doesn't get anywhere near the love it deserves. A couple of years ago I would never have said this but TFS is a pretty amazing tool. I actually have a draft blog post somewhere where I complained about TFS at length. Since I wrote it 2 years ago almost every one of mycomplaintshas been addressed. The only one outstanding after this git thing is around the use of WF in the build set up.

TFS is going to continue to have the TFVC source control engine available to you, should you want it. I'm not sure who is going to be using it or what advantages Microsoft see it having. In my mind TFVC is to the git backend as Silverlight is to HTML5. It is the 6th Sense of source control: dead but it doesn't know it yet.

I've used a lot of source control systems over the years(CVS, Subversion, Perforce, Mercurial, VSS, Harvest, ClearCase,"¦(yikes, I'm old)) and I can honestly say that git is the best of them. However there is a pretty big learning curve with git and git certainly provides sufficient rope to hang yourself with enough left over to hang your extended family. I'm always cautious when one technology is adopted by all the players in a field as "the right way". No problem with the complexity of source control can be fully solved and if you think it can then you don't [understand the problem](http://yunshui.wordpress.com/2008/07/22/marmite-for-peace/).

My concern is that all the major source control players will start doing things the git way and we'll become entrenched in thinking that it is the only solution. We'll spend the next 10 years digging our way out of the hole just as we've done with IoC and ORMs.

I'm cautiously optimistic about the TFS approach to git. Git is better than TFVC but it doesn't follow that git is better than all comers. Keep watching out for innovations in source control, it isn't a solved problem.



