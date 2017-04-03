---
layout: post
title: How is Azure Support?
authorId: simon_timms
date: 2015-03-18
---
From time to time I stumble on an Azure issue I just can't fix. I don't like to rely too heavily on people I know in the Azure space because they shouldn't be punished for knowing me too much (sorry, Tyler). I've never opened a support ticket before and I imagine most others haven't either. This is how the whole thing unrolled:

This time the issue was with database backups. A week or so ago I migrated one of my databases to v12 so I could get some performance improvements. I tested the migration and the performance on a new server so I was confident. I did not, however, think about testing backups. Backups are basic and would have been well tested by Microsoft, right? Turns out that isn't the case. 

The first night my backup failed and, instead of a nice .bacpac file I was left with ten copies of my database. 

![http://i.imgur.com/qzZReCc.png](http://i.imgur.com/qzZReCc.png)

Of course each one of these databases is consuming the same S1 sized slot on the server and is being billed to me at S1 levels. Perhaps more damning was that the automatic backup task seemed to have deleted itself from the portal. I put the task back and waited for the next backup window to hit. I also deleted the extra databases and ran a manual backup. 

When the next backup window hit the same problem reoccurred. This was an issue too deep inside Azure for me to diagnose myself. I ponied up the $30/month for support and logged an issue. I feel like with my MSDN subscription I probably get some support incidents for free but it was taking me so long to find how to use them $30 was cheaper. 

The timeline of the incident was something like 

noon - log incident
3:42 - incident assigned to somebody from Teksystems
3:48 - scope of incident defined
3:52 - incident resolved

This Teksystems dude works fast! I hope all his incidents were as easy to solve as mine. The resolution: "Yeah, automatic backups are broken with v12. We'll fix it at some point in the future. Do manual backups for now"

I actually think that is a pretty reasonable response. I'm not impressed that backups were broken in this way but things break and get fixed all the time. With point in time restore there was no real risk of losing data but it did throw off my usual workflow (download last night's backup every day for development today). 

What I'm upset about is that this whole 4 hour problem could have been prevented by putting this information on the Azure health page. Back in November there was a big Azure failure and one of the lessons Microsoft took away was to do a better job of updating the health dashboard. At least they claimed to have taken that lesson away. From what I can see we're not there yet. If we, as an industry, are going to put our trust in Azure and other cloud providers then we desperately need to have transparency into the status of the system. 

I was once told, in an exit interview, that I needed to do a better job of not volunteering information to customers. To this day I am totally aghast at the idea that we wouldn't share technical details with paying customers. Customers might not care but the willingness to be above board should always be there. The CEO of the company I left is being indicted for fraud which you won’t get if everybody was dedicated to the truth. 

This post has diverged from the original topic of Azure support. My thoughts there are that it is really good. That $30 saved me hours of messing about with backups for days. If I had a lot of stuff running on Azure I would buy the higher support levels which, I suspect, provide an even better level of service.

