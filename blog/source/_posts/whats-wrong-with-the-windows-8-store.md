---
layout: post
title: What's Wrong with the Windows 8 Store
authorId: simon_timms
date: 2013-05-30
---

As I posted [a few days ago](http://blog.simontimms.com/2013/05/28/my-first-windows-8-app/ "My First Windows 8App") I put my first app into the approval process for Windows 8. I actually put it into the approval process on the 21st of May, 9 days ago. It has been rejected 3 times and I'm still waiting for it to be approved. Honestly if it doesn't go this time I'm going to give up and move onto other things. It isn't worth my effort. I feel that most of the blame for this lies with the approval process. This is a rant about what's wrong with the process.

**REJECTED**

My first complaint was that there sure were a lot of fields to be filled in when setting up the app. Somewhere in that mess of fields I forgot to fill in a privacy policy. I guess privacy policies are needed for any app which communicates over the network. Fine. The thing is you need to put it in two places, one in the app and one in the approval metadata. I had forgotten the one in the approval metadata. Okay, okay I added that and tried again.

**REJECTED**

WTF, I got that privacy policy in there. Oh there is a password requirementburiedin the app. I used a template for my first app and didn't realize that there was a password requirement. Mea culpa. I could have supplied a test account but I just disabled that functionality. At 6am when I'm trying to get out to work I didn't care. I'll add them back in version 1.1.

**REJECTED**

Oh come on, now what? Oh it is in the wrong category. What? It is a blog and I put it in books. A blog is sort of like a book. Well I'm sure they gave me guidance as to which category it should be in. Oh, nope they didn't bother with that. Jerks. I suppose I'll guess again since there is zero guidance out there on which category to put things in.

What is pissing me off is that each one of these rejections costs me 3 days of waiting. If they had bothered to test the entire application the first time and written down all the problems then we wouldn't be in this rejection cycle.There is a concept in computing called â€œfail fastâ€ the theory being that if you fail quickly instead of trying to compensate you can get problems solved while they are still small. This isn't a place where that theory should be applied. The long approval cycle time removes allbenefitfrom failing fast. TELL ME EVERYTHING UP FRONT.

<iframe allowfullscreen="" frameborder="0" height="267" src="http://www.youtube.com/embed/yMQhXc1dHIQ?feature=oembed" width="474"></iframe>

That the app was in the wrong category should have been instantly apparent to whatever people they have approving apps. That could have been easily fixed if it had been mentioned in the first rejection, same with the login.

While I'm bitching let me say that I'm sick of signing into the store. Every fricking action needs me to sign in. Run the app through the validation process? Sign in. Create a new app? Sign in. Check on the progress of your app? Sign in. Go to the store? Sign in. Can we maybe come up with a shared key store or at least remember the god damn password within Visual Studio? After developing an app I had to throw out my keyboard as a security risk â€” all the keys used for my password were worn down.


# What can be done to fix it

1. Test the app from end to end. Every. Time. You're putting a bad taste in people's mouth by failing them again and again. Just fail them once and help them out, Jesus.

2. Be more helpful when you fail the app. I'm just guessing that my app is in the correct category now. Who knows. If the tester had told me which category it should be in I wouldn't have had to guess. It would have been a dozen key strokes, help a brother out.

3. Tighten up the release cycle times. 3 days is too long. This isn't 1995 when I had to ship a DVD to some magic DVD mastering center to be put into a package and mailed out to my customers.Sorry I live in a world of instant gratification. I'm sure somebody will comment and tell me how much better the windows store process is than Apple's. I don't care, that's not the point. Apple should fix their stuff too.

4. The sign in stuff. Please, it is a minor annoyance but it sure as hell stacks up on top of the other stuff with which I'm dealing.

5. During the automated testing of the app it should be apparent that the app is talking to a network. You could flag the lack of privacy policy there. It is an easy fix and would have saved me a whole rejection.

6. If you're using approvals/day as metric for the testers stop it. Crossing well into the world of supposition here I bet that some middle manager at Microsoft has looked at the approval process and said â€œwe need to instrument thisâ€. The obvious metric is how many apps a tester looks at in a day. By instrumenting this they are actually encouraging testers to quickly fail things and not test themthoroughly. That isn't thebehavioryou want. Instrument something else.

This is stuff which needs to be done now. Windows 8 is shaping up to be a bigger disaster than Vista and improving theexperiencefor developers should be high on the list of things to do. I don't have to put up with this bullshit when I deploy my app through apt-get or chocolatey nuget. The advantage of having my app in the windows store is pretty small from what I can see. I would rather distribute a classic app through traditional channels than deal with the windows store again.



