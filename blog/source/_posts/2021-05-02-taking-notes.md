---
layout: post
title: Taking Notes
authorId: simon_timms
date: 2021-05-02 10:00

---

Taking notes is hard. I think I took notes in university but I wasn't very good at it. I'd either put everything in them making them unapproachably long or I'd put in too little information. As a result I've kind of shied away from taking notes in my professional career. Unfortunately, is it starting to bite me more and more as I jump around between technologies and projects. I often find myself saying "shoot, I just did this 6 months ago - how do I do that?" 

<!-- more -->

About two months back I decided that this was becoming a way too common occurrence and that something needed to change. I've been seeing some noise about research notes software like [Roam Research](https://roamresearch.com/) and [Obsidian](https://obsidian.md/). Roam Research looked like a lot of vendor lock-in and Obsidian wasn't free if I wanted to synchronize my notes between devices - a must. I don't really have a huge need for the ability to build up backlinks between articles. I mostly want to know what I did on any day and put in small notes for how to do specific things like adding logging to a functions app or how to get Okta to do some esoteric thing. 

I found a great kind of middle ground in [Foam](https://foambubble.github.io/foam/) which is a suite of plugins for VS Code that transform it into a handy little knowledge retention tool. I try to take notes on a daily basis of what I did that day for each of my clients and then jot down notes on anything technical which I think is important. I'm also expanding my notes to include other parts of my life like where I put the lock nut key for my tires which I seem to misplace every time it is time to change the tires on my car. 

My workflow looks like 
1. As soon as I start work I'll create a new daily note. In it I add headings for all the clients I'm working with at the moment. There is an issue to create templates for daily notes which will be nice and will provide me a shortcut when it goes live.
2. Any time I change tasks with a client I'll jot down a quick note about what I'm doing. If I have a meeting with that client I'll sometimes write down the notes from that meeting in my daily notes. 
3. If I encounter something that I think I'll need to again one day I'll write a quick note down about it and I happily include a bunch of screen shots. I categorize these notes into folder, typically based on the technology in use `Azure`, `JavaScript`, ... I also annotate the notes liberally with tags. Foam suggests existing tags so hopefully I don't end up with similar tags like `javascript`,`JavaScript`,`javaScript`. 

Now that I have a couple of months worth of notes stacked up I can just use the standard search tools in VS Code to find them. If I have a problem with the instructions I left myself I'll often expand on the notes with the benefit of hindsight. I've lost track of how many times this has been invaluable to me. I'm hoping this will make me a better developer because I don't have to rely quite so much on my memory for minutia. 

## Taking it online

All my success got me thinking: what if other could benefit from some of these notes? A lot of my notes are hyper-local or have client information in them I'd rather not leak. But a lot of them are generic and might be useful to other people so why not put them up on my blog? They're going to be lower quality than a standard blog post but sometimes it isn't about quality. So I spent a good couple of hours figuring out how I could build a github action to take select articles from my private knowledge base and publish them as public articles. 

Now when I annotate an article with a special secret tag (it's `public`) the script will automatically publish the knowledge base article as a blog post. In fact the first of these just went live at https://blog.simontimms.com/2021/05/02/function-appinsights/

I imagine I've messed some of this up and will probably need to fiddle with it a little over the next few months but for now enjoy the stream of low quality nonsense that my script produces