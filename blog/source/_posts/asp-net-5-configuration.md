---
layout: post
title: ASP.net 5 Configuration
authorId: simon_timms
date: 2014-11-14
---

On twitter yesterday I had a good conversation with [Matt Honeycutt](http://trycatchfail.com) about configuration in ASP.net 5. It started with

> The more I think about it: yeah, putting app-specific connection strings and things in evn variables is a TERRIBLE idea. [#AspNetvNext](https://twitter.com/hashtag/AspNetvNext?src=hash)
> 
> - Matt Honeycutt (@matthoneycutt) [November 13, 2014](https://twitter.com/matthoneycutt/status/532952247530180608)

<script async="" charset="utf-8" src="//platform.twitter.com/widgets.js"></script>

Today I'm seeing a lot more questions about how configuration works in ASP.net <del>vNext</del> 5 (sorry, still getting use to that).

> Hail Hydra! How do I avoid collision with env variables bw apps on the same server? [#AspNetvNext](https://twitter.com/hashtag/AspNetvNext?src=hash)
> 
> - Chris Lawrence (@cnlawrence1183) [November 14, 2014](https://twitter.com/cnlawrence1183/status/533367060743868416)

<script async="" charset="utf-8" src="//platform.twitter.com/widgets.js"></script>

> Isn't it high time to rename [#AJAX](https://twitter.com/hashtag/AJAX?src=hash) to [#AJAJ](https://twitter.com/hashtag/AJAJ?src=hash)? : Everything gone JSON now! Even the configuration files in [#AspNetvNext](https://twitter.com/hashtag/AspNetvNext?src=hash)?
> 
> - Mustakim Ali (@MustakimAli) [November 15, 2014](https://twitter.com/MustakimAli/status/533439991247306752)

<script async="" charset="utf-8" src="//platform.twitter.com/widgets.js"></script>

It sounds like there is some clarification needed about how configuration works in ASP.net 5.

The first thing is that configuration in ASP.net 5 is completly pluggable. You no longer need to rely on the convoluted Web.config file for all your configuration. All the configuration code is found in the [Configuration repository](https://github.com/aspnet/Configuration) on github. You should start by looking at the [Configuration.cs](https://github.com/aspnet/Configuration/blob/dev/src/Microsoft.Framework.ConfigurationModel/Configuration.cs) file, this is the container class that holds the configuration for your application. It is basically a box full of strings. How we get things into that box is the interesting part.

In the standard template for a new ASP.net 5 project you'll find a class called Startup.cs. Within that class is the configuration code

<script src='https://gist.github.com/stimms/c4fc63017db9e8d9a887.js'></script>

In the default configuration we're reading from a json based configuration file and then overriding it with variables taken from the environment. So if you were developing and wanted to enable an option called SendMailToTestServer then you could simply define that in your environment and it would override the value from the json file.

Looking again in the Configuration repository we see that there are a number of other configuration sources such as

- Ini files
- Xml files
- In memory

The interface you need to implement to create your own source is simple and if you just extend ```BaseConfigurationSource``` that should get you most of the way there. So if you want to keep your configuration in [Zookeeper](https://zookeeper.apache.org/) then all you would need to do is implement your own source that could talk to Zookeeper. Certain configuration providers also allow changes in the configuration to be committed back to them.

The next point of confusion I'm seeing is related to how environmental variables work. For the most part .net developers think of environmental variables as being like PATH: you set it once and it is globally set for all processes on that machine. For those people from a more Linuxy/UNIXy background we have a whole different interpretation.

Environment variables are simply pieces of information that are inherited by child processes. So when you go set your PATH variables by right clicking on My Computer in Windows (it is still called that, right?) you're setting a default set of environmental variables that are inherited by all launched processes. You can set them in other ways, though.

Try this: open up two instances of powershell. In the first one type

$env:asp="rocks" echo $env:asp

You should see "rocks" echoed back. This sets an environmental variable and then echos it out. Now let's see if the other instance of powershell has been polluted by this variable. Type

echo $env:asp

Nothing comes back! This is because the environments, after launch, are separate for each process. Now back to the first window and type

start powershell

This should get you a third powershell window. In this one type

echo $env:asp

Ah, now you can see "rocks" echoed again. That's because this new powershell inherited its environment from the parent process.

Environments are NOT global. So you should have no issue running as many instances of ASP.net 5 on your computer as you like without fear of cross polluting them so long as you don't set your variables globally.

Why even bother with environmental variables? Because it is a common language that is spoken by every operating system (maybe not OpenVMS but let's be realists here). It is also already supported by Azure. If you set up a configuration variable in an Azure WebSite then when that is set in the environment. That's how you can easily configure node application or anything else.Finally it helps eliminate that thing where you accidentally alter and check in a configuration file with settings specifically for your computer and break the rest of your team. Instead of altering the default configuration file you could just set up and environment or you could set up a private settings file.

<script src='https://gist.github.com/stimms/47f5c282229d26635f5f.js'></script>

Where AddPrivateJsonFile extends the json configuration source and swallows missing file exceptions allowing your code to work flawlessly on production.

In a non-cloud production environment I would still tend to use a file based configuration systeminstead of environmental variables.

The new configuration system is extensible and powerful. It allows for chaining sources and solves a lot of problems in a more elegant fashion than the old XML based transforms. I love it.



