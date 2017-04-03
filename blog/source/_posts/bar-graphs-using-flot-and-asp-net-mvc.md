---
layout: post
title: Bar Graphs Using Flot and ASP.net MVC
authorId: simon_timms
date: 2009-06-21
---

There are [a](http://blog.rebeccamurphey.com/2007/12/17/graph-table-data-jquery-flot/) [bunch](http://blog.bobpeers.com/tag/flot/) of posts out there about using [flot](http://code.google.com/p/flot/) to create nifty HTML graphs but the Internet is all about reiteration and I need to start posting more frequently. Flot is a javascript library which uses [HTML Canvas](http://en.wikipedia.org/wiki/Canvas_(HTML_element)) from HTML 5 to draw simple vector graphics. It works natively on good browsers and with a little bit of hacking on IE8 too. At its most simple one needs only include the jquery libraries and the flot libraries. Technically the excanvas library needs only be included for IE but it doesn't seem to do any harm to include it all the time and the packed version is just a few kb.

  
<script src="" language="javascript" type="text/javascript">   
  
" language="javascript" type="text/javascript"><script language="javascript" type="text/javascript" src="">

As you can see I always use the ResolveClientURL function to resolve paths to javascript libraries. This section is added to my Site.Master but if your site doesn't include graphs on every page then it would be more efficient to include it only on the pages with graphs. For the purposes of this post we're also going to add

  
<script src="" language="javascript" type="text/javascript">

which adds some extensions to the normal flot package. We're most interested in tick labels on the x-axis. More on that later. Now we need to get some data ready to be graphed. I have a handy function in the repository for fetching a list of data objects, in the controller for the page I call

  
public ActionResult Index()  
 {  
 ViewData["UserActivityTotals"] repository.getUserActivityTotals(user.userID);  
 return View()  
 }

This returns a collection of UserActivityTotal objects which are lightweight objects

```csharp  
namespace ActivityTracker.Models   
{  
 public class UserActivityTotal  
 {  
 public string ActivityName { get; set; }  
 public int TotalPoints { get; set; }  
 }  
}
```
Now in the view I print out this information into javascript

```  
var userActivitiesNames = [<%   
 counter = 1;  
 if(null != ViewData["UserActivityTotals"])  
 {  
 foreach(UserActivityTotal uat in ViewData["UserActivityTotals"] as List)  
 {  
 if(counter > 1)  
 Response.Write(",");  
 Response.Write("[" + counter + ","" + uat.ActivityName + ""]");  
 counter ++;  
 }  
 }  
  
 %>];
 ```

That counter thing is in there because javascript can't handle a trailing comma in array definitions unlike Perl or Ruby. That would be a really nice thing to fix for the next version of javascript. Anyway this results in something which looks like

  
var userActivities = [[0.5, 1899],[1.5, 157],[2.5, 904]];

We're just putting in a monatomically increasing series for the x value. These ensure that the columns don't overlap or have large gaps between them. Why don't we just put in the values which we have in the ActivityName field? Because flot doesn't handle x-axis labels which are not numerical or dates. For that we have to start using the extension to flot which we included earlier. First let's just get the graph onto the page.

Add a div to define the size and location of the graph

  
<div id="UserActivityTotalsGraph" style="width:600px;height:300px;"></div>

and a snippet of javascript which sets up the graph object and assigns it to the div

 ``` 
$.plot($("#UserActivityTotalsGraph"), [userActivities], {  
 label: "Points",  
 series: {  
 data: userActivities,  
 color: "rgb(182,188,194)"  
 },  
 bars: { show: true, barWidth: 1.0 },  
 yaxis: { min: 0 }  
 });
```
[![](http://stimms.files.wordpress.com/2009/06/flotgraph1.jpg?w=300)](http://stimms.files.wordpress.com/2009/06/flotgraph1.jpg)

That looks pretty nice except for the labels on the bottom. To get those working we hop back into the view and add an xaxis line to the plot call.

```
$.plot($("#UserActivityTotalsGraph"), [userActivities], { label: "Points",  
 series: {  
 data: userActivities,  
 color: "rgb(182,188,194)"  
 },  
 bars: { show: true, barWidth: 1.0 },  
 yaxis: { min: 0 },  
 xaxis: { ticks: userActivitiesNames}  
 });
```

This uses another array called userActivitiesNames which we define with

```  
var userActivitiesNames = [<%  
 counter = 1;  
 if(null != ViewData["UserActivityTotals"])  
 {  
 foreach(UserActivityTotal uat in ViewData["UserActivityTotals"] as List)  
 {  
 if(counter > 1)  
 Response.Write(",");  
 Response.Write("[" + counter + ","" + uat.ActivityName + ""]");  
 counter ++;  
 }  
 }  
  
 %>];
```
And we're done! The final product looks like this:

[![](http://stimms.files.wordpress.com/2009/06/flotgraph2.jpg?w=300)](http://stimms.files.wordpress.com/2009/06/flotgraph2.jpg)



