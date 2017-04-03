---
layout: post
title: Bar Graphs Using Flot and ASP.net MVC - Part II
authorId: simon_timms
date: 2009-06-22
---

I wasn't really planning a part II to this article but I left the code in something of a mess and we can do much better than having to write a bunch of javascript each time we need to make a graph. Let's use the HtmlHelper and extension methods. The HtmlHelper is a class in System.Web.Mvc which is often extended to supply shortcuts for writing out HTML. xVal extends it to add client side validation and we're going to do the same thing to write out the javascript for our graphs. We start by creating a new extension class to complement HtmlHelper

  
public static class HTMLExtensions  
 {  
 public static string BarGraph(this HtmlHelper helper, string divName, IDictionary dataPoints)  
 {  
 String dataPointsArrayName = divName + "Values";  
 String dataPointLabelsArrayName = divName + "Labels";  
  
  
 String returnText = @"  
 var {0} = [";   
  
 int counter = 0;  
 foreach (KeyValuePair kvp in dataPoints)  
 {  
  
 if (counter > 0)  
 returnText += ",";  
 returnText += "[" + counter + ".5, " + kvp.Value + "]";  
 counter++;  
 }  
 returnText += "];n";  
  
 returnText += @"var {1} = [";   
 counter = 1;  
  
 foreach (KeyValuePair kvp in dataPoints)  
 {  
 if (counter > 1)  
 returnText += ",";  
 returnText += "[" + counter + ","" + kvp.Key + ""]";  
 counter++;  
 }  
 returnText += "];n";  
  
 returnText += @"$(document).ready(function()   
 {  
 $.plot($(""#{2}""), [{0}], {   
 series: {  
 data: {0},   
 color: ""rgb(182,188,194)""   
 },   
 bars: { show: true, barWidth: 1.0 },   
 yaxis: { min: 0 },  
 xaxis: { ticks: {1}}   
 });  
 });";  
  
 returnText += "";  
  
 returnText = returnText.Replace("{0}",dataPointsArrayName).Replace("{1}", dataPointLabelsArrayName).Replace("{2}", divName);  
  
 return returnText.ToString();  
 }  
  
 }

This class is a bit of a mess of string appending but hopefully changes to it will be infrequent. I know somebody is going to jump on my string appending but before you do take a read of Jeff Atwood's excellent examination of [micro optimization](http://www.codinghorror.com/blog/archives/001218.html). The key take away is that all the messy javascript generation which we did have in the views is now centralized into one place.

So now we have an extension method we can call from our View, so let's pop back over to that. The page has two graphs on it and it use to have a huge amount of hand coded javascript. That can all be replaced with

  
<%Dictionary topTeamsDictionary = (ViewData["TopTeams"] as List).ToDictionary(n => n.team.teamName, n => n.total);  
Dictionary userActivityTotals = (ViewData["UserActivityTotals"] as List).ToDictionary(n => n.ActivityName, n => n.TotalPoints);%>

The data in the ViewData was in a list and that list is need by other things on the page so we quickly transform the List into a Dictionary using LINQ. That is pretty much it. So long as the div referenced in the call to BarGraph exists you should get the same stylish graphs as yesterday. Obviously there are a lot of other options which can be passed through to the graph, these are left as an exercise for the reader. I've always wanted to say that.



