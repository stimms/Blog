---
layout: post
title: Fizz-Buzz Returns
authorId: simon_timms
date: 2013-01-30
---

On twitter today I was noticing that we're getting back into a debate about fizz-bizz or fizz-buzz as a tool for hiring programmers. I blame a combination of [Jimmy Bogart](http://lostechies.com/jimmybogard/2013/01/29/fizzbuzz-is-dead-long-live-fizzbuzz/) and [Rob Connery](http://wekeroad.com/2013/01/28/a-better-interview-question-star-alignment)for starting things up again. Funny how frequently Rob is to blame for things"¦

I love Fizz-Bizz as an interview tool. For those not familiar with it the idea is that you have the candidate write a very simple piece of code the requirements for which are something along the lines of

1. <span style="line-height:14px;">For 100 cycles have the application print</span>

- Fizz if the cycle number is a multiple of 3
- Bizz if the cycle number is a multiple of 5
- The number otherwise

That's it. The output should look something like

> 0  
>  1  
>  2  
>  Fizz  
>  4  
>  Bizz  
>  "¦

The question is a very easy one and most people should be able to write an answer very quickly. I let people write it on the whiteboard, I don't care what language they use but they are time boxed at about 5 minutes.

Some people suggest that this is a form of gotcha interview questions. Meaning that either you come in knowing the answer already or you get lucky and figure it out. I disagree. This is super basic programming. The only tricky part is that it uses modularmathematics but honestly everybody learned that when they did division in grade 3 and had a remainder. Fizz-bizz is not gotcha interviewing as far as I'm concerned because it is exactly what you're going to be doing in the job: figuring out problems and programming solutions.

Obviously fizz-bizz is not the end of your interview questions; answering fizz-bizz should not get you a job. It is the starting point and you use it to filter out the 10% or 15% of people who claim they can program but can't actually. I'm just guessing at the percentages but I have wasted countless hours in interviews with people who really don't understand the craft at all. If I can end the interview after 10 minutes instead of an hour then that is a huge time savings for me.

One incident sticks in my mind where I asked an interviewee to program a fibonacci sequence. In order to avoid it being a trap I explained, carefully, what a fibonacci sequence is and how to define it. We spend the next hour watching the poor thing flounder around trying to solve the problem. My mistake there was to allow the interview to go on for so long. We should have cut and run after 10 minutes.

The dirty secret about interviewing is that you've probably made the decision about hiring the person in the first 10 minutes anyway. First impressions are very important no matter how you try to avoid jumping to decisions. Cognitive biases of which we're not even aware colour our impressions and lead us to seek evidencewhichsupports our initial conclusions. Fizz-bizz has the advantage of being a more empiricaltest than most interview questions. Asking it first, or after a phone interview is ideal. Bogart suggests that it can be done as a take home. I don't like that because I think there is insight to be found in watching somebody complete the test.

During the test we watch out for thought processes and how smoothly the candidate abandons attempts which don't work. It also gives advanced developers the opportunity to show off by using TDD or functional programming to develop the solution. I live for somebody asking me the fibonacci sequence question so I can break out the [closed form of fibonacci](http://en.wikipedia.org/wiki/Fibonacci_number#Closed-form_expression).

Fizz-bizz is a useful filter for saving the interviewer time and it is an indicative tests. There may be times when you eliminate somebody who would have been a good candidate but I don't think it is frequent. I highly recommend using fizz-bizz or a similar question in your interviews.



