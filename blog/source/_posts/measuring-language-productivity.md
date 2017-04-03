---
layout: post
title: Measuring Language Productivity
authorId: simon_timms
date: 2009-12-13
---

I recently asked a question over at stackoverflow about the productivity gains in various languages.

  
Does anybody know of any research or benchmarks of how long it takes to develop the same  
application in a variety of languages? Really I'm looking for Java vs. C++ but any  
comparisons would be useful. I have the feeling there is a section in Code Complete  
about this but my copy is at work.

I was really asking because I wanted to help justify my use of the [Pellet](http://clarkparsia.com/pellet/) semantic reasoner over the [FaCT++](http://owl.man.ac.uk/factplusplus/) reasoner in a paper.

What emerged from the question was that there really was not much good research into the topic of language productivity and that any research which had been done was from the 2000 time-frame. What makes research like this difficult is finding a large sample size and finding problems which don't favour one class of language greatly over another. That got me thinking, what better source of programmers is there than stackoverflow? There are developers from all across the spectrum of languages and abilities; there is even a pretty good geographic disbursement.

Let's do this research ourselves! I propose a stackoverflow language programming contest. We'll develop a suite of programming tasks which try as hard as possible to not focus on the advantages of one particular language and gather metrics. I think we should gather

- Time taken to develop
- Lines of code required
- Runtime over the same input
- Memory usage over the same input
- Other things I haven't thought of

I'll set up a site to gather people's solutions to the problems and collate statistics but the problems should be proposed by the community. We'll allow people to checkout the problem set, time how long it takes to the to complete it and then submit the code for their answers. I'll run the code and benchmark the results and after, say two weeks of having the contest open, publish my results as well as the dataset for anybody else to analyze.



