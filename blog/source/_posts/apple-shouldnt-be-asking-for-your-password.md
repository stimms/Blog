---
layout: post
title: Apple Shouldn't be Asking for Your Password
authorId: simon_timms
date: 2015-02-12
---
My macbook decided that yesterday was a good day to become inhabited by the spirits of trackpads past and just fire random events. When I bought this machine I also bought apple care, which, as it turns out was a really, really good idea. It cost me something like $350 and has, so far, saved me

<table style="width: 300px">
<tbody>
<tr>
<td>Power adapter</td>
<td>$100</td>
</tr>
<tr>
<td>
Trackpad
</td>
<td>$459</td>
</tr>
<tr>
<td>Screen</td>
<td>$642</td>
</tr>
<tbody>
<tfooter style="background: #f3f3f3">
<tr>
<td>Total</td>
<td>$1201</td>
</tr>
</tfooter>
</table>

In the process of handing my laptop over for the latest round of repairs the apple genius asked for my user name and password. 

I blinked. 

"You want what?"

The genius explained that to check that everything was working properly they would need to log in and check things like sound and network. This is, frankly, nonsense. There is no reason that the tech should need to log into the system to perform tests of this nature. In the unlikely case that the sophisticated diagnostic tools they have at their disposal couldn't check the system then it should be standard procedure to boot into a temporary, in memory, version of OSX. 

When I pushed back they said I could create a guest account on the machine. This is an okay solution but it still presents opportunity to leverage local privilege escalation exploits, [should](http://www.securityweek.com/mac-os-x-affected-critical-rootpipe-privilege-escalation-vulnerability) [they](https://www.trustedsec.com/august-2013/osx-10-8-4-local-root-privilege-escalation-exploit/) [exist](https://code.google.com/p/google-security-research/issues/detail?id=121). It is certainly not unusual for computer techs to [steal data](http://consumerist.com/2013/08/13/geek-squad-accused-of-stealing-distributing-customers-naked-photos-yes-again/) from the computers they are servicing. Why give Apple that opportunity? 

What I find more disturbing is that a large computer company that should know better is teaching people that it is okay to give out password. It isn't. If there were a 10 commandments of computer security then 

    Thou shalt not give out thy password
    
Would be very close to the top of the list. At risk is not just the integrity of that computer but also of all the passwords stored on that computer. How many people have chrome save their passwords for them? Or have active sessions that could be taken over by an attacker with their computer password? Or use the same password on many systems or sites? I don't think a one of us could claim that none of these apply to them. 

I don't know why Apple would want to take on the liability of knowing people's passwords. When people, even my wife, offer to give me their passwords I run from the room, fingers in ears screaming "LA LA LA LA LA" because I don't want to know. If something goes wrong I want to be above suspicion. If there is some other way of performing the task without knowing the password then I'll take it, even if it is slightly harder. 

Apple, this is such an easy policy change, please stop telling people it is okay to give out their password. Use a live CD instead or, if it is a software problem, sit with the customer while you fix it. Don't break the security commandments, nothing good can come of it.

