---
layout: post
title: Adding TypeScript to an Existing Project
authorId: simon_timms
date: 2013-03-11
---

If you read this blog with any sort of frequency you'll know that I'm somewhere in the range of 15-18% about typescript. In my travels the other day I created a new web project on a machine without typescript installed(go on, ask me how many computers I have at home). When I went to add typescript to the project it didn't compile and I squinted my third hardest squint. Digging around on the web I found that one might need to add a target to the .csproj file. The examples I found all pointed to the typescript found in C:program files.

I hate that.

Doing so means that you have to install typescript, in the default location, for the compile to work. That's too much friction for setting up the project on a new machine. Instead I copied the contents of that directory into a tools directory in my project and checked it in. Now when people compile it they won't even notice that it is building their typescript for them and the friction for a new developer is 0.

The target? Well just paste this into your .csproj file right before the </project>

<script src='https://gist.github.com/stimms/5120387.js'></script>

I put typescript in a tools/typescript directory at the same level as my .sln file. All good to go.



