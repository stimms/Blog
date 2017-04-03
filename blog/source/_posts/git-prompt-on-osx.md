---
layout: post
title: Git prompt on OSX
authorId: simon_timms
date: 2014-09-15
---

I have a bunch of work to do using git on OSX over the next few months and I figured it was about time I changed my prompt to be git aware. I'm really used to having this on Windows thanks to the excellent [posh git](https://github.com/dahlbyk/posh-git). If you haven't used it in the prompt you get the branch you're on, the number of files added, modified and deleted as well as a color hint about the state of your branch as compared with the upstream (ahead, in sync, behind).

[![Screen Shot 2014-09-11 at 10.27.43 PM](http://stimms.files.wordpress.com/2014/09/screen-shot-2014-09-11-at-10-27-43-pm.jpg?w=300)](https://stimms.files.wordpress.com/2014/09/screen-shot-2014-09-11-at-10-27-43-pm.jpg)

It is wonderful. I wanted it on OSX. There are actually quite a few tutorials that will get you 90% of the way there. I read one by [Mike O'Brein](http://www.mikeobrien.net/blog/osx-git-bash-prompt/)but I had some issues with it. For some reason the brew installation on my machine didn't include git-prompt. It is possible that nobody's does"¦ clearly a conspiracy. Anyway I found a copy over at the [git repository on github](https://github.com/git/git/blob/master/contrib/completion/git-prompt.sh). I put it into my home directory and sourced it in my .profile.

if [ -f $(brew --prefix)/etc/bash_completion ]; then . $(brew --prefix)/etc/bash_completion fi source ~/.git-prompt PS1="33[32m]@ 33[33m]w$(__git_ps1 " (33[36m]%s33[33m])") n$33[0m] "

This got me some of the way there. I had the branch I was on and it was coloured for the relationship to upstream but it was lacking any information on added, removed and modified files.

[![Screen Shot 2014-09-11 at 10.52.10 PM](http://stimms.files.wordpress.com/2014/09/screen-shot-2014-09-11-at-10-52-10-pm.jpg?w=300)](https://stimms.files.wordpress.com/2014/09/screen-shot-2014-09-11-at-10-52-10-pm.jpg)

So I cracked open the .git-profile and got to work. I'll say that it has been a good 5 years since I've done any serious bash scripting and it is way worse than I remember. I would actually have go this done in powershell in half the time and with half the confusion as bash. It doesn't help that, for some reason, people who write scripts feel the need to use single letter variables. Come on, people, it isn't a competition about brevity.

I started by creating 3 new variables

local modified="$(git status | grep 'modified:' | wc -l | cut -f 8 -d ' ')" local deleted="$(git status | grep 'deleted:' | wc -l | cut -f 8 -d ' ')" local added="$(git ls-files --others --exclude-standard | wc -l | cut -f 8 -d ' ')"

The first two make use of git status. I had a quick twitter chat with Adam Dymitruk who suggested not using git status as it was slow. I did some bench marking and tried a few other commands and indeed found that it was about twice as expensive to use git status as to use git diff-files. I ended up replacing these variables with the less readable

local modified="$(git diff-files|cut -d ' ' -f 5|cut -f 1|grep M|wc -l| cut -f 8 -d ' ')" local deleted="$(git diff-files|cut -d ' ' -f 5|cut -f 1|grep D|wc -l | cut -f 8 -d ' ')" local added="$(git ls-files --others --exclude-standard | wc -l | cut -f 8 -d ' ')"

Chaining commands is fun!

Once I had those variables in place I changed the gitstring in .git-prompt to read

local gitstring="$c$b${f:+$z$f}$r$p [+$added ~$modified -$deleted]"

See how pleasant and out of place those 3 new variables are?

I also took the liberty of changing the prompt in the .profile to eliminate the new line

PS1="33[32m]@ 33[33m]w$(__git_ps1 " (33[36m]%s33[33m])") $33[0m] "

My prompt ended up looking like

[![Screen Shot 2014-09-12 at 6.59.55 AM](http://stimms.files.wordpress.com/2014/09/screen-shot-2014-09-12-at-6-59-55-am.jpg?w=300)](https://stimms.files.wordpress.com/2014/09/screen-shot-2014-09-12-at-6-59-55-am.jpg)

Beautiful. Wish I'd done this far sooner.



