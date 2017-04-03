---
layout: post
title: Bookmarklet for MSDN
authorId: simon_timms
date: 2009-08-19
---

Today I was cruising the old MSDN using their much better [low bandwidth](http://www.hanselman.com/blog/LowBandwidthViewAndOtherHiddenAndFutureFeaturesOfMSDN.aspx) version when I stumbled across [a page on events in C#](http://msdn.microsoft.com/en-us/library/aa645739%28VS.71,loband%29.aspx). What got my attention was the example code all grey and boring not to mention hard to follow. What this page needed was a little bit of [SyntaxHighlighter](http://alexgorbatchev.com/wiki/SyntaxHighlighter) Alex Gorbatchev's glorious javascript library which add syntax to source code. I use it right here on my blog as does everybody else who passes the coolness test. The test, of course, being the use of SyntaxHighlighter. I hacked at the jQueryify bookmarklet and managed to get it to load the correct SyntaxHighlighter libraries and stylesheets. The next time you're on MSDN squinting at a piece of code try hitting MSDN Style. <del>It only works on firefox at the moment but I'll update it to work on IE as well.</del> Simply drag this link to your bookmarks bar and you're good to go:

[MSDN Style]()


## Source

  
javascript:%20(function(){  
 function getScript(url,success){  
 var script=document.createElement('script');  
 script.src=url;  
 var head=document.getElementsByTagName("head")[0], done=false;  
 script.onload=script.onreadystatechange = function(){  
 if ( !done && (!this.readyState ||  
 this.readyState == "loaded" || this.readyState == "complete") ) {  
 done=true;  
 success();  
 }  
 };  
 head.appendChild(script);  
 };  
 getScript('http://alexgorbatchev.com/pub/sh/2.0.320/scripts/shCore.js',function(){});  
 getScript('http://alexgorbatchev.com/pub/sh/2.0.320/scripts/shBrushCSharp.js',function(){});  
 getScript('http://ajax.googleapis.com/ajax/libs/jquery/1/jquery.min.js',function() {  
 loadStyle('http://alexgorbatchev.com/pub/sh/2.0.320/styles/shCore.css');  
 loadStyle('http://alexgorbatchev.com/pub/sh/2.0.320/styles/shThemeDefault.css');  
 return completeLoad();  
 });  
 function loadStyle(url)  
 {  
 style=document.createElement('link');  
 style.href = url;  
 style.rel = "stylesheet";  
 style.type="text/css";  
 document.getElementsByTagName("head")[0].appendChild(style);  
 };  
  
 function completeLoad() {  
 $(".libCScode:not(div)").addClass("brush: csharp");  
  
 SyntaxHighlighter.highlight();  
 };  
})();



