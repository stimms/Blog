---
layout: post
title: Importing a git Repository into TFS
authorId: simon_timms
date: 2013-04-02
---

From time to time there is need to replace a good technology with a not so good technology. The typical reason is a business one. I don't claim to have deep understanding of how businesses works but if you find yourself in a situation where you need to replace your best friend, your amigo, your confident Git with the womanizing, drunken lout that is TFS then this post is for you! This post describes how to import a git repo into TFS and preserve most of the history.

The first think you'll need is a tool called Git-TF which can be found on [codeplex](http://gittf.codeplex.com/). This comes as a zip file and you can unzip it anywhere. Next you'll need to add the unzip directory to your path. If you're just doing this as a one-time operation then you can use the powershell command:

`$env:Path += ";C:tempgit-tf"`

to add to your path.

Now that you have that git should be able to find a whole set of new subcommands. You can check if it is working by running

git tf

You should get a list of subcommands you can run

<div class="wp-caption aligncenter" id="attachment_2528" style="width: 760px">[![Git-TF subcommands](http://stimms.files.wordpress.com/2013/04/powershell-git-tf.jpg)](http://stimms.files.wordpress.com/2013/04/powershell-git-tf.jpg)Git-TF subcommands

</div>Now drop into the git repository you want to push to TFS and enter

git tf configure http://tfsserver:8080/tfs $/Scanner/Main

Wherehttp://tfsserver:8080/tfs is the collection path for your TFS server and$/Scanner/Main is the server path to which you're pushing. This will modify your .git/config file and add the following:

[git-tf "server"] collection =http://tfsserver:8080/tfs serverpath =$/Scanner/Main

Your git repository now knows a bit about TFS. All you need to do now is push your git code up and that can be done using

git tf checkin --deep

This will push all the commits on the mainline of your git repo up into TFS. Without the "“deep flag only the latest commit will be submitted.

There are a couple of gotchas around branching. You may get this error:

git-tf: cannot check in - commit 70350fb has multiple parents, please rebase to form a linear history or use --shallow or --autosquash

You canflattenyour branches by either rebasing or by passing git-tf the "“autosquash flag which will attempt to flatten the branching structure automatically. I'm told that autosquashing can consume a lot of memory if there are a lot of commits in the repository. I have not had any issue but my repositories are small and my machine has 16GB of memory.

Now you have move all your source code over to TFS. Yay.

I'm not going to point out that if you keep git-tf around you can continue to work as if you have git and just push commits to TFS. That would likely be against your company's policies.



