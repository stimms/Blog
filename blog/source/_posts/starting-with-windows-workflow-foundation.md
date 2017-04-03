---
layout: post
title: Starting with Windows Workflow Foundation
authorId: simon_timms
date: 2013-02-07
---

I happened to be talking to a friend of mine who was looking for some advice about how to deal with a generalization problem he was having. His problem was around invoices, as so many problems seem to be. His software supports a single workflow for the processing of invoices. It is a tried and tested workflow but he wanted to be able to offer his clients some configurability around the workflow.

I suggested that he take a look a a workflow engine. Workflow engines are systems which allow for a rules based approach to the processing of actions. Basically it is a state machine with actions which can occur in each state. They can usually be modeled as a flow diagram complete with branches and loops. I don't usually suggest using workflow engines; in my experience they're over complicated and far more trouble than they're worth. However a lot of my reservations are due to the ways in which people make use of them.

Frequently developers include them in their applications to allow users to define their work processes. The idea is that in an ever changing business you shouldn't need involve those developer guys to change your workflow. Besides, those developers are very smart: they just sit around and debate the relative merits of Wheel of Time and Song of Ice and Fire. A workflow engine empowers the business to make their own changes. There is a whole class of workflow engines designed to manage business processes. Heck there is even a language called Business Process Execution Language or BPEL which can be fed into these engines by the end users as they figure out their workflow. At least that is the theory.

In my experience workflows become complicated very quickly, growing beyond the ability of business people to manage. I'm not saying that business people are stupid just the their expertise lie elsewhere. Simon's rule about workflows is much the same as my rule about reporting: **any workflow sufficiently complicated to be useful is too complicated for users to manage themselves**. If the users aren't going to be building the workflow then you're likely going to recompile and redeploy yourapplicationanyway so one of the big advantages of using a workflow engine is gone. Workflows violate the idea of keeping programming as simple as possible.

So why am I recommending them? In this case my friend wasn't going to let users write their own workflows and his workflows were very simple. His current flow iscapturedin a C# file of no more than 100 lines.

There are a ton of workflow engines out there but I suggested using Windows Workflow Foundation which is, confusingly, known as WF. I've used it before to describe builds in TFS but never in one of my own applications. The information out there on WF is a bit confusing as there are tutorials about version 3.5 and some about version 4 the two of which are as different as pasta and electromagnativity. This getting started guide is all about version 4.

Start with a blank console application. Add the WF libraries to the project. These come with .net 4 so you don't have to hunt them down.

<div class="wp-caption aligncenter" id="attachment_2241" style="width: 635px">[![Workflow Libraries](http://stimms.files.wordpress.com/2013/02/screen-shot-2013-02-06-at-10-26-47-pm.png)](http://stimms.files.wordpress.com/2013/02/screen-shot-2013-02-06-at-10-26-47-pm.png)Workflow Libraries

</div>Next you create an *Activity*. Activity are the core elements of a workflow. An activity can contain other action. You can build your own custom activities or use one of the built in activities. The built in activities are not particularly useful other than the tasks related to the flow through the workflow such as if and while.

[![Screen Shot 2013-02-06 at 10.27.17 PM](http://stimms.files.wordpress.com/2013/02/screen-shot-2013-02-06-at-10-27-17-pm.png)](http://stimms.files.wordpress.com/2013/02/screen-shot-2013-02-06-at-10-27-17-pm.png)You should now have a blank canvas in front of you to which you can add activities. I added two code activities. The first was a simple console activity to write string to the console

<script src='https://gist.github.com/stimms/4728816.js'></script>

It is pretty basic class. The only interesting piece is

public InArgument<string> Message { get; set; }

This property allows you to pass information in and out of the activity from the rest of the workflow. Obviously this is an input argument. I used an output argument in my other task:

<script src='https://gist.github.com/stimms/4728844.js'></script>

This simply returns a random number. From these two tasks and the built in If activity I was able to make this simple activity for the all important task of cheese counting

<div class="wp-caption aligncenter" id="attachment_2242" style="width: 635px">[![Simple workflow](http://stimms.files.wordpress.com/2013/02/screen-shot-2013-02-06-at-10-48-56-pm.png)](http://stimms.files.wordpress.com/2013/02/screen-shot-2013-02-06-at-10-48-56-pm.png)Simple workflow

</div>The workflow itself contains a variable called NumberOfCheeses which is scoped to the sequence. The output of GetARandomNumber is assigned to this variable

<div class="wp-caption aligncenter" id="attachment_2243" style="width: 538px">[![Activity properties](http://stimms.files.wordpress.com/2013/02/screen-shot-2013-02-06-at-10-52-24-pm.png)](http://stimms.files.wordpress.com/2013/02/screen-shot-2013-02-06-at-10-52-24-pm.png)Activity properties

</div>I also set the input properties of the two WriteToConsole activities

<div class="wp-caption aligncenter" id="attachment_2244" style="width: 546px">[![What will be printed](http://stimms.files.wordpress.com/2013/02/screen-shot-2013-02-06-at-10-59-43-pm.png)](http://stimms.files.wordpress.com/2013/02/screen-shot-2013-02-06-at-10-59-43-pm.png)What will be printed

</div>This simple workflow should be sufficient to test that we've grasped the concepts. How do we run it? Workflows can be run in a number of ways, they can be handed off to standalone workflow engines or you can use a self hosted workflow runtime. I chose the second one as it was far easier.

<script src='https://gist.github.com/stimms/4728871.js'></script>

Here a new workflow invoker is created and given the main activity shown above. It is then started. If we execute the application and check out the console, the output from the WriteToConsole activity is shown. Easy peasy!

There is a lot more to workflow such as the ability to load and persist workflows from databases and hook into events in the workflow. I may do another blog once I've figured some of those out.



