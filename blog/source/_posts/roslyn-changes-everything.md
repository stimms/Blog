---
layout: post
title: Roslyn Changes Everything
authorId: simon_timms
date: 2014-04-04
---

Yesterday at Microsoft's build conference there was a huge announcement: Microsoft were open sourcing their new C#/VB.net compiler. On the surface this seems like a pretty minor thing. I mean who looks at how compilers work? "This is probably going to be interesting to academics who study compilers and nobody else."

Well I disagree. I think it is going to be a huge turning point in how programmers work with code.

There are other open source compilers: GCC,LLVM both come to mind as great examples. The differences between these and Roslyn are huge. First Roslyn is a much more modern compiler than almost anything else out there. I still think of clang, which is based on LLVM, as the new kid on the block, however LLVM was started in 2000: 14 years ago. Roslyn was written from the ground up over the last 4 years. I haven't looked but I would bet that it makes much better use of things like parallel processing than other compilers. There is a pretty vague post on the [C# blog](http://blogs.msdn.com/b/csharpfaq/archive/2014/01/15/roslyn-performance-matt-gertz.aspx)about how they're treating performance of the compiler as a feature. Idon't know what progress they've made on that front but we'll certainly be seeing some benchmarks come out in the next few weeks as people dig into Roslyn.

Next is that Roslyn written in a much more accessible language: C#. It is going to be far easier for the average developer to jump into modifying the compiler than it would be add some functionality to LLVM. Roslyn was designed to be an extensible compiler. It has a well defined API and some phenomenal extension points into which people can plug. I think that we're going to see a huge number of plugable modules which mutate the language.

<div class="wp-caption aligncenter" id="attachment_3288" style="width: 760px">[![The build pipeline for Roslyn taken from the overview on codeplex http://roslyn.codeplex.com/wikipage?title=Overview&referringTitle=Home](http://stimms.files.wordpress.com/2014/04/pipeline.png)](http://stimms.files.wordpress.com/2014/04/pipeline.png)The build pipeline for Roslyn taken from the overview on codeplex http://roslyn.codeplex.com/wikipage?title=Overview&referringTitle=Home

</div>Finally I'm excited that Roslyn will enable smaller, more incremental changes to the languages it compiles. Already we're seeing some hints as to this. In the [Tour of Roslyn](http://blogs.msdn.com/b/csharpfaq/archive/2014/04/03/taking-a-tour-of-roslyn.aspx) post there was an example of inline declarations:

 public static void Main(string[] args) { if (int.TryParse(args[0], out var n1)) { Console.WriteLine(n1); } }

There is support for these in Roslyn but not in the classic C# compiler. Little things like this are going to add up and make the language much better. If shipping for Roslyn can be decoupled from Visual Studio, a given for open source projects, then we can see awesome new features enabled rapidly instead of waiting for the full releases of Visual Studio.




# What can we do with the compiler?

Here are some quick ideas I had about what we could plug into Roslyn. Some of them are mad dreams but some of them are almost certain to get made.


## Aspect Weaver

There is already and AOP weaver available for the .net platform in Aspect Sharp. It has a bit of a reputation for being slow. It works by rewriting the IL instructions which is kind of hacky and presents some problems. With Roslyn there should be no need to hook into the build that late. I think you could manipulate the syntax tree to inject calls to the aspects whenever needed.

[![syntax tree](http://stimms.files.wordpress.com/2014/04/syntax-tree.png)](http://stimms.files.wordpress.com/2014/04/syntax-tree.png)



AOP should be vastly easier and may even be more powerful with this syntax tree rewriting.




## Custom Compiler Errors

Is there some practice you're trying to avoid in your team? Perhaps long methods are really a huge deal for you and you want to fail the build when some mouth-breather writes a method which is over 50 lines long. No problem! Just plug into the syntax tree API and fail the compile when long methods are detected. Perhaps you want to check for and fail on concatenating strings and them running them against a database(SQL injection). Again this could be plugged in without a great deal of trouble.


## Domain Specific Languages

There are plenty of nifty places where it would be fun to be able to define a custom syntax for certain projects. Perhaps you're writing a message based system and you want to make it easier to write message handlers. With some Roslyn work a new syntax could be added so that instead of writing

<script src='https://gist.github.com/stimms/9974513.js'></script>

You could just write

<script src='https://gist.github.com/stimms/9974534.js'></script>

and all the wireup would be dealt with by the compiler.


## Random other Syntax Improvements

You know what syntax I really like? Post if statements. I think they're nifty and read more like human language.

<script src='https://gist.github.com/stimms/9974694.js'></script>

This the sort of thing which can just be added by rewritingthe syntax tree. Oh or how about cleaning up the accessors for collections?

<script src='https://gist.github.com/stimms/9974761.js'></script>

That's probably a terrible syntax now I think about it"¦ whatever it is still possible.


# It is going to be awesome!

I envision a future where any project of appreciable size will include a collection of syntax and compiler modules. These will be compiled first, plugged into Roslyn and then used to build the rest of the project. Coding standards will be easier to enforce, compilations will be more powerful. There is a risk that the language proliferation will get out of hand but I'm betting it will settle down after 2 or 3 years and we'll get a handful of new dialects out of this. There will need to be new tooling developed to make changing compilers in VS easier. Package managers like nuget will need to be updated to support compiler modules but that seems trivial.

It is an exciting time to be a .net developer. I'm so glad that when I had the option I decided to go down the .net path and not the Java path. Those suckers just got lambdas and we're working with the most modern, flexible, extensible compiler in the world? No contest.



