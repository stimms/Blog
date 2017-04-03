---
layout: post
title: Best Bug I've Written Today
authorId: simon_timms
date: 2009-02-28
---

Today I create a handy little class in my attempt to rewrite wordle. It encapsulated a word and a word count and implemented comparable

<span style="color:#9a9a9a;"> 1 </span><span style="color:#ff3030;font-weight:bold;">package</span> com<span style="color:#555555;">.</span>simontimms<span style="color:#555555;">.</span>wordle<span style="color:#555555;">;</span>  
<span style="color:#9a9a9a;"> 2 </span>  
<span style="color:#9a9a9a;"> 3 </span><span style="color:#ff3030;font-weight:bold;">public class</span> WordCountElement <span style="color:#ff3030;font-weight:bold;">implements</span><span style="color:#0000ff;">Comparable</span><span style="color:#555555;"><</span>WordCountElement<span style="color:#555555;">>{</span>  
<span style="color:#9a9a9a;"> 4 </span>  
<span style="color:#9a9a9a;"> 5 </span>  
<span style="color:#9a9a9a;"> 6 </span><span style="color:#ff3030;font-weight:bold;">private</span><span style="color:#f48c23;">int</span> count <span style="color:#555555;">=</span><span style="color:#32ba06;">0</span><span style="color:#555555;">;</span>  
<span style="color:#9a9a9a;"> 7 </span><span style="color:#ff3030;font-weight:bold;">private</span><span style="color:#0000ff;">String</span> word<span style="color:#555555;">;</span>  
<span style="color:#9a9a9a;"> 8 </span>  
<span style="color:#9a9a9a;"> 9 </span>  
<span style="color:#9a9a9a;"> 10 </span><span style="color:#ff3030;font-weight:bold;">public</span><span style="color:#d11ced;">WordCountElement</span><span style="color:#555555;">(</span><span style="color:#0000ff;">String</span> word<span style="color:#555555;">,</span><span style="color:#f48c23;">int</span> count<span style="color:#555555;">)</span>  
<span style="color:#9a9a9a;"> 11 </span><span style="color:#555555;">{</span>  
<span style="color:#9a9a9a;"> 12 </span><span style="color:#ff3030;font-weight:bold;">this</span><span style="color:#555555;">.</span>word <span style="color:#555555;">=</span> word<span style="color:#555555;">;</span>  
<span style="color:#9a9a9a;"> 13 </span><span style="color:#ff3030;font-weight:bold;">this</span><span style="color:#555555;">.</span>count <span style="color:#555555;">=</span> count<span style="color:#555555;">;</span>  
<span style="color:#9a9a9a;"> 14 </span><span style="color:#555555;">}</span>  
<span style="color:#9a9a9a;"> 15 </span>  
<span style="color:#9a9a9a;"> 16 </span><span style="color:#ff3030;font-weight:bold;">public</span><span style="color:#f48c23;">int</span><span style="color:#d11ced;">compareTo</span><span style="color:#555555;">(</span>WordCountElement toCompare<span style="color:#555555;">)</span>  
<span style="color:#9a9a9a;"> 17 </span><span style="color:#555555;">{</span>  
<span style="color:#9a9a9a;"> 18 </span><span style="color:#ff3030;font-weight:bold;">return this</span><span style="color:#555555;">.</span>count <span style="color:#555555;">-</span> toCompare<span style="color:#555555;">.</span><span style="color:#d11ced;">getCount</span><span style="color:#555555;">();</span>  
<span style="color:#9a9a9a;"> 19 </span>  
<span style="color:#9a9a9a;"> 20 </span><span style="color:#555555;">}</span>  
<span style="color:#9a9a9a;"> 21 </span>  
<span style="color:#9a9a9a;"> 22 </span><span style="color:#ff3030;font-weight:bold;">public</span><span style="color:#f48c23;">void</span><span style="color:#d11ced;">setCount</span><span style="color:#555555;">(</span><span style="color:#f48c23;">int</span> count<span style="color:#555555;">) {</span>  
<span style="color:#9a9a9a;"> 23 </span><span style="color:#ff3030;font-weight:bold;">this</span><span style="color:#555555;">.</span>count <span style="color:#555555;">=</span> count<span style="color:#555555;">;</span>  
<span style="color:#9a9a9a;"> 24 </span><span style="color:#555555;">}</span>  
<span style="color:#9a9a9a;"> 25 </span>  
<span style="color:#9a9a9a;"> 26 </span><span style="color:#ff3030;font-weight:bold;">public</span><span style="color:#f48c23;">int</span><span style="color:#d11ced;">getCount</span><span style="color:#555555;">() {</span>  
<span style="color:#9a9a9a;"> 27 </span><span style="color:#ff3030;font-weight:bold;">return</span> count<span style="color:#555555;">;</span>  
<span style="color:#9a9a9a;"> 28 </span><span style="color:#555555;">}</span>  
<span style="color:#9a9a9a;"> 29 </span>  
<span style="color:#9a9a9a;"> 30 </span><span style="color:#ff3030;font-weight:bold;">public</span><span style="color:#f48c23;">void</span><span style="color:#d11ced;">setWord</span><span style="color:#555555;">(</span><span style="color:#0000ff;">String</span> word<span style="color:#555555;">) {</span>  
<span style="color:#9a9a9a;"> 31 </span><span style="color:#ff3030;font-weight:bold;">this</span><span style="color:#555555;">.</span>word <span style="color:#555555;">=</span> word<span style="color:#555555;">;</span>  
<span style="color:#9a9a9a;"> 32 </span><span style="color:#555555;">}</span>  
<span style="color:#9a9a9a;"> 33 </span>  
<span style="color:#9a9a9a;"> 34 </span><span style="color:#ff3030;font-weight:bold;">public</span><span style="color:#0000ff;">String</span><span style="color:#d11ced;">getWord</span><span style="color:#555555;">() {</span>  
<span style="color:#9a9a9a;"> 35 </span><span style="color:#ff3030;font-weight:bold;">return</span> word<span style="color:#555555;">;</span>  
<span style="color:#9a9a9a;"> 36 </span><span style="color:#555555;">}</span>  
<span style="color:#9a9a9a;"> 37 </span><span style="color:#555555;">}</span>

At another point in my code I created a sorted set out of these WordCountElements

<span style="color:#9a9a9a;"> 1 </span><span style="color:#ff3030;font-weight:bold;">public</span><span style="color:#0000ff;">TreeSet</span><span style="color:#555555;"><</span>WordCountElement<span style="color:#555555;">></span><span style="color:#d11ced;">getSortedSetOfWordCounts</span><span style="color:#555555;">(</span><span style="color:#0000ff;">String</span> textToAnalyze<span style="color:#555555;">)</span>  
<span style="color:#9a9a9a;"> 2 </span><span style="color:#555555;">{</span>  
<span style="color:#9a9a9a;"> 3 </span><span style="color:#0000ff;">HashMap</span><span style="color:#555555;"><</span><span style="color:#0000ff;">String</span><span style="color:#555555;">,</span> WordCountElement<span style="color:#555555;">></span> unsortedSet <span style="color:#555555;">=</span><span style="color:#d11ced;">getWordCounts</span><span style="color:#555555;">(</span>textToAnalyze<span style="color:#555555;">);</span>  
<span style="color:#9a9a9a;"> 4 </span><span style="color:#0000ff;">TreeSet</span><span style="color:#555555;"><</span>WordCountElement<span style="color:#555555;">></span> sortedSet <span style="color:#555555;">=</span><span style="color:#ff3030;font-weight:bold;">new</span><span style="color:#0000ff;">TreeSet</span><span style="color:#555555;"><</span>WordCountElement<span style="color:#555555;">>();</span>  
<span style="color:#9a9a9a;"> 5 </span>  
<span style="color:#9a9a9a;"> 6 </span><span style="color:#ff3030;font-weight:bold;">for</span><span style="color:#555555;">(</span><span style="color:#0000ff;">String</span> s <span style="color:#555555;">:</span> unsortedSet<span style="color:#555555;">.</span><span style="color:#d11ced;">keySet</span><span style="color:#555555;">())</span>  
<span style="color:#9a9a9a;"> 7 </span><span style="color:#555555;">{</span>  
<span style="color:#9a9a9a;"> 8 </span> sortedSet<span style="color:#555555;">.</span><span style="color:#d11ced;">add</span><span style="color:#555555;">(</span>unsortedSet<span style="color:#555555;">.</span><span style="color:#d11ced;">get</span><span style="color:#555555;">(</span>s<span style="color:#555555;">));</span>  
<span style="color:#9a9a9a;"> 9 </span><span style="color:#0000ff;">System</span><span style="color:#555555;">.</span>out<span style="color:#555555;">.</span><span style="color:#d11ced;">println</span><span style="color:#555555;">(</span>s<span style="color:#555555;">);</span>  
<span style="color:#9a9a9a;"> 10 </span><span style="color:#555555;">}</span>  
<span style="color:#9a9a9a;"> 11 </span><span style="color:#ff3030;font-weight:bold;">return</span> sortedSet<span style="color:#555555;">;</span>  
<span style="color:#9a9a9a;"> 12 </span><span style="color:#555555;">}</span>

I couldn't get my unit tests to pass when I had more than one word in the unsortedSet. I ended up debugging it and found that even though I was adding two different words only the first was present in the sorted set. Well [select might not be broken](http://www.pragprog.com/the-pragmatic-programmer/extracts/tips) but perhaps sorted sets were. Can you see the bug?

That's right, the custom comparator resulted in two words with equal counts being equal. So select wasn't broken. Drat.



