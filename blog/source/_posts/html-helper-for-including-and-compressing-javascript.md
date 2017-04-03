---
layout: post
title: HTML Helper for Including and Compressing Javascript
authorId: simon_timms
date: 2009-06-23
---

<span style="background-color:#F1425B;">This article is outdated and ill advised</span>

If you're like me then you probably have a whole bunch of javascript includes on your site cluttering up the top of your page. I tend to keep most of mine in Site.Master, it isn't so much that I use them all on every page but I do make use of a large enough subset that I can't be bothered to load them in each view. For example here is what I have at the top of a Site.Master page for my Activity Tracker project:

  
 <script src="" language="javascript" type="text/javascript">   
 <script src="" language="javascript" type="text/javascript">   
 <script src="" language="javascript" type='text/javascript' >   
 <script src="" language="javascript" type='text/javascript' >   
 <script src="" language="javascript" type='text/javascript' >   
  
  
 <script src="" language="javascript" type="text/javascript">  
 <script src="" language="javascript" type="text/javascript">  
  
  
 <script src="" language="javascript" type="text/javascript">   
 <script src="" language="javascript" type="text/javascript">   
 <script src="" language="javascript" type="text/javascript">   
 <script src="" language="javascript" type="text/javascript">

This thing was not only a mess but it was also amazingly inefficient. Each time the web browser opens a connection to the server there is overhead, if you put everything in one file then you can save on this overhead. Apparently you can only have [two open connections](http://www.ajaxperformance.com/2006/12/18/circumventing-browser-connection-limits-for-fun-and-profit/) per domain so you don't really get any advantage from having multiple .js file which run parallel. I was starting to notice the overhead just opening up pages from my localhost. Things would get bad if I deployed this to the real world. I went through a couple of possible solutions:

1. Manually combine the javascript files, they change infrequently   
2. Combine the javascript during the build and produce just one file  
3. Use edge caches of the javascript files

Manual combination of the files was out right away, I don't like doing work over and over again that is what computers are for. I liked the idea of 2 because I'm a huge [build guy](http://www.amazon.com/Build-Master-Microsofts-Configuration-Addison-Wesley/dp/0321332059) and to do that I would get to play with msbuild or nant or something like that. However I liked being able to make javascript change without rebuilding the site.

Solution #3, using edge caches, seemed like a great plan. The issue is that these systems are not always reliable, not from a always up point of view but from a changes point of view. I couldn't govern when they made changes to the scripts and I would have to use stock versions of all the scripts. The autocomplete script I used is slightly modified for my purposes so #3 was out.

I decided that what I needed was a home grown solution so I came up with Html.IncludeScriptLibraries. This is a new extension method (I'm in to those at the moment) which loaded javascript files for me.

  
 public static string IncludeScriptLibraries(this HtmlHelper helper, string basePath, params string[] scripts)  
 {  
 String contents = "";  
 foreach(String script in scripts)  
 {  
 String fileName = basePath + "\" + script.Replace("/", "\");  
 if (File.Exists(fileName))  
 {  
 StreamReader reader = File.OpenText(fileName);  
 contents += reader.ReadToEnd();  
 reader.Close(); }  
 }  
 return "" + contents + "";  
 }

This simple function opens a list of script files from some directory and concatenates them into one string. This is then returned. The master page now has

  
<%= Html.IncludeScriptLibraries(Server.MapPath(ResolveClientUrl("~/Scripts")),   
 "jquery-1.3.2.js",  
 "jquery.tablesorter.min.js",  
 "extjs/ext-jquery-adapter.js",   
 "extjs/ext-all-debug.js",  
 "jquery.autocomplete/js/jquery.autocomplete.js",  
 "xVal.jquery.validate.js",  
 "jquery.validate/jquery.validate.js",  
 "flot/jquery.flot.js",  
 "flot/excanvas.js",  
 "flot/ext.flot.js",  
 "jquery-ui-1.7.1.js"  
 )%>

Pretty nifty. Now we have this library inclusion abstracted into a function we can add some other cool stuff. I grabbed a .net port of the pack javascript library from [Dean Edwards ](http://dean.edwards.name/packer/) and turned it into a library in my project. I changed the extension method to return

  
ECMAScriptPacker packer = new ECMAScriptPacker();  
return "" + packer.Pack(contents) + "";

Which should have nicely packed up the files. However the library seems to munge javascript as does the sample application, which is a pitty. If it had worked that would be pretty cool.

This extension method is a way to centralize the loading of javascript onto a pages and provides the ability to preprocess it through compression or anything else. A caching level should be added to avoid hitting the disk with every query.

As always I'm curious about possible improvements to this.



