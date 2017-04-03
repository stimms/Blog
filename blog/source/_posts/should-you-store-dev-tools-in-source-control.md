---
layout: post
title: Should you store dev tools in source control?
authorId: simon_timms
date: 2014-01-02
---

The utility of storing build tools and 3rd party libraries in source control is an age old question. One that I have flipflopped on for a number of years. There are arguments on each side of checking things in.

<table><thead><tr><th></th><th>Check in</th><th>Install separately</th></tr></thead><thead><tr><td>Size of repository</td><td>Increases rapidly, time required for a fresh checkout gets long</td><td>Remains the same regardless of how much is installed</td></tr><tr><td>Reproducibility of builds</td><td>Good</td><td>Poor</td></tr><tr><td>Ease of spinning up new developers or build machines</td><td>Easy</td><td>Difficult, possibly impossible</td></tr><tr><td>Ease of setting up initial builds</td><td>Difficult, many build tools do not like being checked in. Paths may not work and dependent libraries are difficult to find.</td><td>No increased difficulty</td></tr></thead></table>There is no easy answer one way or another.

If I had to set up repositories and builds on a new project today using one of these approaches I think I would look at a number of factors to decide what to do

- Lifetime of the project. Projects which have a shorter life would not see as much churn in build tools and libraries so would not benefit as much from checking in tooling. Shorter projects are also less likely to see a number of new developers join the team mid-project. New developers would have to spend time setting up their workstations to match other workstations and build servers.
- Distribution of the team. Developers working in remote locations are going to have a harder time setting up builds without somebody standing over their shoulder.
- Number of other projects on which the developers are working. If developers are working on more than one project then they may very well run into conflicts between the current project and other projects.
- Lifespan of the code. Projects which have a long projected life will, of course, have changes in the future. If the project isn't under continual development during that time then it is advisable to check in the development tools so that future developers can easily build and deploy the code.
- The project is using unique tools or environments. For instance if the development shop is typically a .net shop but this project called for the development of an iPhone application then check in the tools.

The major disadvantages to checking in binaries are really that it slows down the checkout on the CI server and that checking in some binaries is difficult. For instance I remember a build of the Sun Studio compiler on a now very outdated version of Solaris was very particular about having been installed in /usr/local/bin. I don't remember the solution now but it involved much fist shaking and questioning the sanity of the compiler developers.

There are now a few of new options for creating build environments which can vastly improve these issues: virtual machines, configuration management tools and package managers. Virtual machines are glorious in that they can be set up once and then distributed to the entire team. If the project is dormant then the virtual machines can be archived and restored in the future. Virtual machine hypervisors tend to be pretty good at maintaining a consistent interface allowing for older virtual machines images to be used in newer environments. There are also a lot of tools in place to convert one image format into another so you need not fret greatly about which hypervisor you use.

Configuration management tools such as [puppet](http://puppetlabs.com/), [chef](http://www.getchef.com/chef/), [chocolatey](http://chocolatey.org/) and [vagrant](http://vagrantup.com) have made creating reproducible environments trivial. Instead of checking in the development tools you can simply check in the configuration and apply it over a machine image. Of course this requires that the components remain available somewhere so it might be advisable to set up a local repository of important packages. However this repository can exist outside of source control. It is even possible that, if your file system supports it, you can put symbolic links back to the libraries instead of copying them locally. It is worth experimenting to see if this optimization actually improves the speed of builds.

Package managers such as nuget, npm and gem fulfill the same role as configuration management tools but for libraries. In the past I've found this to fall down a bit when I have to build my own versions of certain libraries (I'm looking at you SharpTestsEx). I've previously checked these into a lib folder. However it is a far better idea to run your own internal repository of these specially modified packages "“ assuming they remain fairly static. If they change frequently then including their source in your project could be a better approach.

Being able to reproduce builds in a reliable fashion and have the ability to pull out a long mothballed project are more important than we like to admit. Unfortunately not all of us can work on greenfield projects at all time so it remains important to make the experience of building old projects as simple as possible. In today's fast moving world a project which is only two or three years old could well be written with techniques or tools which we would now consider comically outdated.

**Edit**: Donald Belcham pointed out to me that I hadn't talked about pre-built VMs

> [@stimms](https://twitter.com/stimms) you forgot pre-built development (or project) specific VMs
> 
> "” Donald Belcham (@dbelcham) [January 2, 2014](https://twitter.com/dbelcham/status/418784127609892864)

<script async="" charset="utf-8" src="//platform.twitter.com/widgets.js"></script>

It is a valid approach but it isn't one I really like. The idea here is that you build a VM for development or production and everything uses that. When changes are needed you update the template and push it out. I think this approach encourages being sloppy. The VMs will be configured but a lot of different people and will get a lot of different changes to them over the lifetime of a project. Instead of having a self-documenting process as chef or puppet would provide you have a mystery box. If there is a change to something basic like the operating system then there is no clear path to get back to an operating image using the new OS. With a configuration management tool you can typically just apply the same script to the new operating system and get close to a working environment. In the long run it is little better than just building snowflake servers on top of bare metal.



