---
layout: post
title: ASP.net MVC returning JSONP
authorId: simon_timms
date: 2009-04-18
---

I've been working on a piece of code which returns [JSON ](http://www.json.org/)in response to some request. It has all being going fine on the local host but I've just deployed it up to my new test server and started to access it using JQuery and it stooped working. This is, of course, typical. This time however, my problem was ignorance rather than a programming blunder. As it turns out when returning JSON across domains you need to actually return JSONP in order to get call backs working. What is [JSONP](http://www.west-wind.com/Weblog/posts/107136.aspx)? I'm glad you asked, because I had no idea either. Basically it is the same JSON you love but wrapped with a bit of text which specifies the name of the function to call upon returning.

<span style="font-weight:bold;">JSON</span>:

{"userID":"00000000-0000-0000-0000-000000000000"³,"success":"false"}

<span style="font-weight:bold;">JSONP</span>:

somefunction({"userID":"00000000-0000-0000-0000-000000000000"³,"success":"false"})

Easy enough. In JQuery you need to just add another parameter to the JSON call in order to pass the name of the function to the server

$<span style="color:#808030;">.</span>getJSON<span style="color:#808030;">(</span>URL <span style="color:#808030;">+</span><span style="color:#0000e6;">"/json/Message/sendMessage?userName="</span><span style="color:#808030;">+</span> $<span style="color:#808030;">(</span><span style="color:#0000e6;">"#userName3"</span><span style="color:#808030;">)</span><span style="color:#808030;">.</span>val<span style="color:#808030;">(</span><span style="color:#808030;">)</span><span style="color:#808030;">+</span>  
<span style="color:#0000e6;">"&messageText="</span><span style="color:#808030;">+</span> $<span style="color:#808030;">(</span><span style="color:#0000e6;">"#message"</span><span style="color:#808030;">)</span><span style="color:#808030;">.</span>val<span style="color:#808030;">(</span><span style="color:#808030;">)</span><span style="color:#808030;">+</span>  
<span style="color:#0000e6;">"&userKey="</span><span style="color:#808030;">+</span> key <span style="color:#808030;">+</span>  
<span style="color:#0000e6;">"&jsoncallback=?"</span><span style="color:#808030;">,</span>  
<span style="color:#800000;font-weight:bold;">function</span><span style="color:#808030;">(</span>json<span style="color:#808030;">)</span>  
<span style="color:#800080;">{</span>  
<span style="color:#800080;">}</span>  
<span style="color:#808030;">)</span><span style="color:#800080;">;</span>

JQuery will automatically replace the ? with the name of your callback function, in this case an anonymous function.

Having discovered all of this I realized that I now had a ton of code returning JsonResults which needed to be changed. I figured the best way to do this was to actually create a JsonpResult which was based off of the JsonResult. So I did just that, basing it off of the now open sourced [ASP.net MVC JsonResult](http://aspnet.codeplex.com/Wiki/View.aspx?title=MVC)

<span style="color:#800000;font-weight:bold;">public</span><span style="color:#800000;font-weight:bold;">class</span> JsonpResult <span style="color:#808030;">:</span> System<span style="color:#808030;">.</span>Web<span style="color:#808030;">.</span>Mvc<span style="color:#808030;">.</span>JsonResult  
<span style="color:#800080;">{</span>  
<span style="color:#800000;font-weight:bold;">public</span><span style="color:#800000;font-weight:bold;">override</span><span style="color:#800000;font-weight:bold;">void</span> ExecuteResult<span style="color:#808030;">(</span>ControllerContext context<span style="color:#808030;">)</span>  
<span style="color:#800080;">{</span>  
<span style="color:#800000;font-weight:bold;">if</span><span style="color:#808030;">(</span>context <span style="color:#808030;">=</span><span style="color:#808030;">=</span><span style="color:#800000;font-weight:bold;">null</span><span style="color:#808030;">)</span>  
<span style="color:#800080;">{</span>  
<span style="color:#800000;font-weight:bold;">throw</span><span style="color:#800000;font-weight:bold;">new</span> ArgumentNullException<span style="color:#808030;">(</span><span style="color:#800000;">"</span><span style="color:#0000e6;">context</span><span style="color:#800000;">"</span><span style="color:#808030;">)</span><span style="color:#800080;">;</span>  
<span style="color:#800080;">}</span>  
  
 HttpResponseBase response <span style="color:#808030;">=</span> context<span style="color:#808030;">.</span>HttpContext<span style="color:#808030;">.</span>Response<span style="color:#800080;">;</span>  
  
<span style="color:#800000;font-weight:bold;">if</span><span style="color:#808030;">(</span><span style="color:#808030;">!</span>String<span style="color:#808030;">.</span>IsNullOrEmpty<span style="color:#808030;">(</span>ContentType<span style="color:#808030;">)</span><span style="color:#808030;">)</span>  
<span style="color:#800080;">{</span>  
 response<span style="color:#808030;">.</span>ContentType <span style="color:#808030;">=</span> ContentType<span style="color:#800080;">;</span>  
<span style="color:#800080;">}</span>  
<span style="color:#800000;font-weight:bold;">else</span>  
<span style="color:#800080;">{</span>  
 response<span style="color:#808030;">.</span>ContentType <span style="color:#808030;">=</span><span style="color:#800000;">"</span><span style="color:#0000e6;">application/json</span><span style="color:#800000;">"</span><span style="color:#800080;">;</span>  
<span style="color:#800080;">}</span>  
<span style="color:#800000;font-weight:bold;">if</span><span style="color:#808030;">(</span>ContentEncoding <span style="color:#808030;">!</span><span style="color:#808030;">=</span><span style="color:#800000;font-weight:bold;">null</span><span style="color:#808030;">)</span>  
<span style="color:#800080;">{</span>  
 response<span style="color:#808030;">.</span>ContentEncoding <span style="color:#808030;">=</span> ContentEncoding<span style="color:#800080;">;</span>  
<span style="color:#800080;">}</span>  
<span style="color:#800000;font-weight:bold;">if</span><span style="color:#808030;">(</span>Data <span style="color:#808030;">!</span><span style="color:#808030;">=</span><span style="color:#800000;font-weight:bold;">null</span><span style="color:#808030;">)</span>  
<span style="color:#800080;">{</span>  
<span style="color:#696969;">// The JavaScriptSerializer type was marked as obsolete prior to .NET Framework 3.5 SP1</span>  
#pragma warning disable <span style="color:#008c00;">0618</span>  
 HttpRequestBase request <span style="color:#808030;">=</span> context<span style="color:#808030;">.</span>HttpContext<span style="color:#808030;">.</span>Request<span style="color:#800080;">;</span>  
  
 JavaScriptSerializer serializer <span style="color:#808030;">=</span><span style="color:#800000;font-weight:bold;">new</span> JavaScriptSerializer<span style="color:#808030;">(</span><span style="color:#808030;">)</span><span style="color:#800080;">;</span>  
<span style="color:#800000;font-weight:bold;">if</span><span style="color:#808030;">(</span><span style="color:#800000;font-weight:bold;">null</span><span style="color:#808030;">!</span><span style="color:#808030;">=</span> request<span style="color:#808030;">.</span>Params<span style="color:#808030;">[</span><span style="color:#800000;">"</span><span style="color:#0000e6;">jsoncallback</span><span style="color:#800000;">"</span><span style="color:#808030;">]</span><span style="color:#808030;">)</span>  
 response<span style="color:#808030;">.</span>Write<span style="color:#808030;">(</span>request<span style="color:#808030;">.</span>Params<span style="color:#808030;">[</span><span style="color:#800000;">"</span><span style="color:#0000e6;">jsoncallback</span><span style="color:#800000;">"</span><span style="color:#808030;">]</span><span style="color:#808030;">+</span><span style="color:#800000;">"</span><span style="color:#0000e6;">(</span><span style="color:#800000;">"</span><span style="color:#808030;">+</span> serializer<span style="color:#808030;">.</span>Serialize<span style="color:#808030;">(</span>Data<span style="color:#808030;">)</span><span style="color:#808030;">+</span><span style="color:#800000;">"</span><span style="color:#0000e6;">)</span><span style="color:#800000;">"</span><span style="color:#808030;">)</span><span style="color:#800080;">;</span>  
<span style="color:#800000;font-weight:bold;">else</span>  
 response<span style="color:#808030;">.</span>Write<span style="color:#808030;">(</span>serializer<span style="color:#808030;">.</span>Serialize<span style="color:#808030;">(</span>Data<span style="color:#808030;">)</span><span style="color:#808030;">)</span><span style="color:#800080;">;</span>  
#pragma warning restore <span style="color:#008c00;">0618</span>  
<span style="color:#800080;">}</span>  
<span style="color:#800080;">}</span>  
<span style="color:#800080;">}</span>

I then extended Controller

<span style="color:#800000;font-weight:bold;">public</span><span style="color:#800000;font-weight:bold;">class</span> WopsleController <span style="color:#808030;">:</span> Controller  
<span style="color:#800080;">{</span>  
<span style="color:#800000;font-weight:bold;">protected</span><span style="color:#800000;font-weight:bold;">internal</span> JsonpResult Jsonp<span style="color:#808030;">(</span><span style="color:#800000;font-weight:bold;">object</span> data<span style="color:#808030;">)</span>  
<span style="color:#800080;">{</span>  
<span style="color:#800000;font-weight:bold;">return</span> Jsonp<span style="color:#808030;">(</span>data<span style="color:#808030;">,</span><span style="color:#800000;font-weight:bold;">null</span><span style="color:#696969;">/* contentType */</span><span style="color:#808030;">)</span><span style="color:#800080;">;</span>  
<span style="color:#800080;">}</span>  
  
<span style="color:#800000;font-weight:bold;">protected</span><span style="color:#800000;font-weight:bold;">internal</span> JsonpResult Jsonp<span style="color:#808030;">(</span><span style="color:#800000;font-weight:bold;">object</span> data<span style="color:#808030;">,</span><span style="color:#800000;font-weight:bold;">string</span> contentType<span style="color:#808030;">)</span>  
<span style="color:#800080;">{</span>  
<span style="color:#800000;font-weight:bold;">return</span> Jsonp<span style="color:#808030;">(</span>data<span style="color:#808030;">,</span> contentType<span style="color:#808030;">,</span><span style="color:#800000;font-weight:bold;">null</span><span style="color:#808030;">)</span><span style="color:#800080;">;</span>  
<span style="color:#800080;">}</span>  
  
<span style="color:#800000;font-weight:bold;">protected</span><span style="color:#800000;font-weight:bold;">internal</span><span style="color:#800000;font-weight:bold;">virtual</span> JsonpResult Jsonp<span style="color:#808030;">(</span><span style="color:#800000;font-weight:bold;">object</span> data<span style="color:#808030;">,</span><span style="color:#800000;font-weight:bold;">string</span> contentType<span style="color:#808030;">,</span> Encoding contentEncoding<span style="color:#808030;">)</span>  
<span style="color:#800080;">{</span>  
<span style="color:#800000;font-weight:bold;">return</span><span style="color:#800000;font-weight:bold;">new</span> JsonpResult  
<span style="color:#800080;">{</span>  
 Data <span style="color:#808030;">=</span> data<span style="color:#808030;">,</span>  
 ContentType <span style="color:#808030;">=</span> contentType<span style="color:#808030;">,</span>  
 ContentEncoding <span style="color:#808030;">=</span> contentEncoding  
<span style="color:#800080;">}</span><span style="color:#800080;">;</span>  
<span style="color:#800080;">}</span>  
  
<span style="color:#800080;">}</span>

and altered all my controllers to extend WopsleController rather than Controller. Seems to work pretty well.



