---
layout: post
title: The Skytech Security Fiasco
authorId: simon_timms
date: 2013-01-22
---

There was a story making the rounds today on the twitter about a Montreal university student who had been expelled for, ostensibly, testing the security of a web site. If you missed it there are a number of articles out there about it as it has become a bit of a media darling.

- [<span style="line-height:14px;">National Post</span>](http://news.nationalpost.com/2013/01/20/youth-expelled-from-montreal-college-after-finding-sloppy-coding-that-compromised-security-of-250000-students-personal-data/)
- [CBC1](http://www.cbc.ca/news/technology/story/2013/01/21/montreal-dawson-college-hack-hamed-al-khabaz.html?autoplay=true)
- [CBC2](http://www.cbc.ca/news/technology/story/2013/01/21/montreal-dawson-college-hack-hamed-al-khabaz.html)

The story goes that this young fellow was working on an app for letting students access their data. In order to test their app they were given access to a test server atSkytech, the company behind the student information software. While playing around he discovered an exploit which allowed him to gain access to information on any student. It is a pretty common exploit: not cleaning your inputs. Al-Khabaz did the right thing in reporting thevulnerabilityand, to their credit, Skytech had a fix deployed in about a day. This is a bit slow in my mind for such a serious exploit but many company aren't quite there yet on being able to deploy at the drop of a hat.

A few days laterAl-Khabaz ran a security testing tool against the test server he had been given to ensure that there were no other vulnerabilities. This is where things start to go off the rails. Skytech noticed an increased load and claim that the attack was damaging their ability to serve their customers. The president of Skytech, Edouard Taza, called upAl-Khabaz and demanded that he come into the Skytech office and sign a non-disclosure agreement or they would press charges.[![Bully](http://stimms.files.wordpress.com/2013/01/bully.gif?w=300)](http://stimms.files.wordpress.com/2013/01/bully.gif)It seems that Dawson College got wind of all this activity and started their own investigation. Theyconveneda pannel of 15 computer science professors who voted to expelAl-Khabaz.

That brings us to today. I see a number of things here which could have been done better both from a technical and from a human relations point of view:

1. There is no denying itAl-Khabaz should have checked with Skytech before running vulnerability tests. I can see where he is coming from and it is unlikely that he knew how much traffic the tool,[Acunetix](http://www.acunetix.com/), would generate on Skytech's site.

2. There is no way that Acunetix, running on a single developer workstation, should be able to take out a website designed to serve such a large body as all the students in Quebec. There is a lack of preparedness for attacks on Skytech's part. This is a site which is likely to attract attacks as it contains a lot of student data including SIN numbers, grades, addresses and the like. One thing is for sure now that Skytech'sineptitudehas been revealed they're going to be the brunt of some actually serious attacks. If you're a student in Quebec you should be worried.

3. An attack on a testing server should not have had an effect on the production site. It is a test server for a reason, you test things against it and, from time to time, that testing is going to be destructive. Separate your servers! With the low prices of cloud servers there is no excuse to have your test site on your production hardware.

4. Skytech reacted well to the first vulnerability but they reacted terribly to the proceeding attacks. As a company you have to know that threatening students with legal action is basically blackmail. If you want people to keep quiet about how crummy your security is then you're pretty much going about it the right way. If you want to actually be secure then you're screwing up. Believe me having a whitehat test your site and report problems is going to save you some big trouble in the future. That's why Google runcompetitionsto find exploits in Chrome.

Now I understand that Skytech have made some moves to fix their screw-ups here including giving Al-Khabaz a scholarship and offering him a job. Good for them. I don't believe he took the job but I wouldn't either, who wants to work for bullies?

5. From what I can tell Skytech were getting a free app created here by students of Dawson. So there was probably some sort of an agreement between Dawson and Skytech to allow students access to a real world system in return for an app. Sounds a lot like slave labour to me. I'm not a fan of unpaid internships or freecollaborations. Companies should pay for apps to bedevelopedfor them. Programmers should not be giving services away for free to companies, it devalues the profession. If you're a programmer and you want to hack on something to help people there is a whole lot of open government data out there which has a greater potential than Skytech's data.

6. Dawson college are so far into the wrong that they can't be saved. To me the fact that 15 researchers chose to slam the research of a student and in fact expel him is crazy to me. They claim that it is against professional conduct. Okay fine, point me to theaccrediteddocument which outlines the professional conduct for a computer scientist. No, no I'll wait.

Exactly.

Even if such a document existed testing the security of a test server is unlikely to be a serious violation. The CBC checked with some lawyers and they could find no charge under the criminal code so it is radically presumptive of the university to suppose that the activities were illegal.

The kangaroo courts that universities set up in this country need to be stopped. These professors, locked in their ivory towers, have no idea about real worldconsequences. Where are the police charges ifAl-Khabaz actually did something seriously wrong?

I know that if I were a student I wouldn't want to go to Dawson College and if I were an employer I would be suspicious of graduates of Dawson. If their professors can't understand the difference betweencriminalhacking and harmless testing they shouldn't be teaching and their students might need remedial training.

Dawson saw this as an optics problem and did what they could to get rid of it. Well that worked out pretty well didn't it, Dawson?

Idiots.



