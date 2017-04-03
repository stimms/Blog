---
layout: post
title: Parsing Command Line Arguments in C#
authorId: simon_timms
date: 2014-07-09
---

If you have the need to parse command line arguments in C# then might I recommend the excellent [Command Line Parser](http://commandline.codeplex.com/). It can be installed from nuget by simply running

 Install-Package CommandLineParser

Once you have it installed you start by setting up an options file which contains properties for all the options you would like your application to understand. Mine looks like

<script src='https://gist.github.com/stimms/5c4c51d721e9ba1253c0.js'></script>

For each parameter you would like parsed you can decorate it with an Option attribute. Within this I defined the shortoptions name(u for export users and h for help) followed by the long option name. I also set the help text, required status and default value for each option.

Within the Main method of my application I called out to the option parser like so

<script src='https://gist.github.com/stimms/39f391f893e0d45adc6e.js'></script>

The help text is particularly useful as you can now easily print print out help which is always a bit of a pain to maintain otherwise.[![commandline](http://stimms.files.wordpress.com/2014/07/commandline.png)](https://stimms.files.wordpress.com/2014/07/commandline.png)



Another nifty feature of the library is the ability to define subcommands as part of your options. This allows building a command line interface similar to git in which the behaviour of the tool changes drastically based on the first undashed option:

git remoteadd http://some.url/project.git

This is quite well documented on the [github wiki](https://github.com/gsscoder/commandline/wiki/Verb-Commands). I haven't tried it yet as my needs are not that complex.

There is a version 2.0 of the library under development on [github](https://github.com/gsscoder/commandline)which changes the interface pretty drastically. There doesn't seem to be a whole lot of development on it so I've opted for the more stable version for my projects.

Overall I would recommend this library over Mono.Options which I've used with a high degree of success in the past.



