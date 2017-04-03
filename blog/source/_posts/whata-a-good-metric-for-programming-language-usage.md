---
layout: post
title: What'a a good metric for programming language usage?
authorId: simon_timms
date: 2013-09-25
---

The whole â€œwhich programming language is most popularâ€ debate was kicked off in my mind today by a tweet from [@kellabyte](https://twitter.com/kellabyte). She tweeted

> â€œX is deadâ€ usually derived from small samples of our industry. [http://t.co/0XFN0RQBrI](http://t.co/0XFN0RQBrI) greater growth than JS in last 12mo. Think about that
> 
> â€” Kelly Sommers (@kellabyte) [September 25, 2013](https://twitter.com/kellabyte/status/382895878547058690)

<script async="" charset="utf-8" src="//platform.twitter.com/widgets.js"></script>

I was outraged that a well respected blogger/tweeter such as kellabyte would tweet horrific lies of this sort. â€œThis is exactlyâ€, I thought, â€œthe problem with our industry â€“ too many people corrupted by fame and supporting their own visual basic.net related agendas.â€ Of course I was wrong: kellabyte has no interested in VB and her numbers were not wrong.

I have always relied on TIOBE's [measurement of programming language popularity](http://www.tiobe.com/index.php/content/paperinfo/tpci/index.html)to give me an idea of what the top languages are. I think this is likely kellabyte's source also. The methodology used is quite extensively outlined at[http://www.tiobe.com/index.php/content/paperinfo/tpci/tpci_definition.htm](http://www.tiobe.com/index.php/content/paperinfo/tpci/tpci_definition.htm). If you don't fancy reading all that the gist is that they use a series of search engines and count the number of results. The ebb and flow of these numbers is what makes up the rankings.

Obviously there are a number of flaws in this methodology:

1. The algorithms used by the search engines are not static
2. Not all programming languages are equally likely to be written about
3. Languages and technologies are often conflated

Let's look at each one of those. The search engine market is a constantly changing landscape. Google and Bing are always working towards improving ranking and how results are reported. There is going to be some necessary churn around ranking changes. TIOBE average out a number of search engines in the hopes they can normalize that problem. They use 23 different search engines which is a good number but many of them are very specialized search engines such as Deviant Art. Certain search engines are also given higher ranking for instance Google gives 28% of the final score. In fact the top 3 search engines account for 69% of the score. I'm no statistician but that doesn't seem like a good distribution. Interestingly 4 out of the top 5 sources are Google properties with the 5th(wikipedia) being heavily sponsored by Google.

The second point is that programming langues are not all equally likely to be written about. My feeling is that newer languages and â€œcoolerâ€ languages will gain an unfair advantage here. People are much more likely to be blogging about them than something boring like VBA. I would say half the code I've written in the last 6 months has been VBA but I don't believe I have more than 2 blog posts on that topic.

I'm guilty of this: when I talk about .net in most cases I'm really talking about C#. Equally when people talk about Rails they're talking about ruby. I'm not convinced that this information is well captured in TIOBE. It is a difficult problem because a search for â€œrailsâ€ is likely to return far more hits than just those related to programming. Context is important and without some natural language processing capabilities I don't see how TIOBE can be accurate.

The alternatives to TIOBE are not particularly promising. [James McKay](http://twitter.com/jammycakes)suggested that looking at job posting and github project would be a better metric. He specifically mentioned the job aggregator[http://www.itjobswatch.co.uk/](http://www.itjobswatch.co.uk/). I've been thinking about this and it seems like a pretty good metric. The majority of development is likely done inside companies so looking at a job site gives a window into the inner workings of companies. Where it falls down is in looking at companies which are too small to post jobs and open source software. The counter balance to that is found in github statistics. These statistics are likely to have the opposite bias favoring upstart languages and open source contributions. I think we're at the point where if you're running an open source project you're running it on github which makes it an invaluable source of data.

To the mix I would add stackoverflow as a source of numbers. They are a large enough question and answer site now that they're a great source of data. I'm not sure what the biases would be there â€“ C# perhaps?

Combining these statistics would be an interesting exercise â€“ perhaps one for a quickly approaching winter's day.



