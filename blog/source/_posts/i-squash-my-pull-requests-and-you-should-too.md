---
layout: post
title: I squash my pull requests and you should too
authorId: simon_timms
date: 2016-02-18
---
A couple of weeks ago I made a change to my life. It was one of those big, earth shattering changes which ripple though everything: I started to squash the commits in my pull requests.

It was a drastic change but I think an overdue one. The reasons for my change are pretty simple: it makes looking at commit histories and maintaining long-lived branches easier. Before my pull requests would contain a lot of clutter: I'd check in small bits of work when I got them working and whoever was reviewing the pull request would have to look at a bunch of commits, some of which would later be reversed, to get an idea for what was going on. By squashing the commits down to a single commit I can focus on the key parts of the pull request without people having to see the mess I generated in between. 

If you have long lived branches (I know, I know) then having a smaller number of commits during rebasing is a real help. There are fewer things that need merging so you don't end up fixing the same change over and over again. 

Finally the smaller number of commits in mainline give a clearer view of what has changed in the destination branch. You individual commits might just have messages like "fixing logging" but when squashed into a PR the commit becomes "Adding new functionality to layout roads automatically". Looking back that "fixing logging" commit isn't at all helpful once you're no longer in the thick of the feature. 

# What has it done for me?

I've already talked about some of the great benefits in the code base but for me, individually, there were some nicities too. First off is that I don't have to worry about screwing up so much. If I go down some crazy path in the code which doesn't work then I can bail out of it easily and without worrying that other developers on my team (Hi, Nick!) will think less of me. 

I find myself checking in code a lot more frequently. I have a git alias to just do 

````
git wip
```


and that checks in whatever I have lying around. It is a very powerful undo feature. It do a git wip whenever I find myself at the completion of some logical step be it writing a unit test or finishing some function or changing some style.  

# How do you do it?

It is easy. Just work as you normally would but with the added freedoms I mentioned. When you're ready to create a pull request then you can issue

````
git log
```

and find the first commit in your branch. There are some suggested ways to do this automatically but they never seem to work for me so I just do it manually. 

Now you can rebase the commits

```
git rebase -i 0ec9df23
```

where 0ec9df23 is the sha of the last commit on the parent branch. This will open up an editor showing all the commits in chronological order. On left you'll see the word pick. 

```
pick 78fc6bc added PrDC session to speaking data
pick 9741725 Podcast with Yves Goeleven

# Rebase 9d792c2..9741725 onto 9d792c2
```

Starting at the bottom just change all but the first of these to `squash` or simply `s`. Save the file and exit. Git will now chug a bit and merge all the changes into one. With this complete you can push to the central repo and issue the pull request. You can add additional changes to this PR to address comments and, just before you do the merge, do another squash. You may need to push with -f but that's okay, this time. 

I'm a big fan of this approach and I hope you will be too. It is better for your sanity, for the team’s sanity and for the git history's sanity.

