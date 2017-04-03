---
layout: post
title: Do you really want "bank grade" security in your SSL? Canadian edition
authorId: simon_timms
date: 2015-05-09
---
In the past few days I've seen a few really [interesting](http://blog.robiii.nl/2015/05/do-you-really-want-bank-grade-security.html) [posts](http://www.troyhunt.com/2015/05/do-you-really-want-bank-grade-security.html) about having bank grade security. I was intersted in them because I frequently tell my clients that the SSL certificates I've got them and the security I've set up for them are as good as what they use when they log into a bank. 

As it turns out I've been wrong about that: the security I've configured is better than their bank. The crux of the matter is that simply picking an SSL cert and applying it is not sufficient to have good security. There are certain configuration steps that must be taken to avoid using old cyphers or weak signatures. 

There are some great tools out there to test if your SSL is set up properly. I like SSL Labs' [test suite](https://www.ssllabs.com/ssltest/index.html). Let's try running those tools against Canada's five big banks. 

<table style="font-size:12px">
	<thead>
    	<tr>
        	<th>Bank</th>
            <th>Grade</th>
            <th>SSL 3</th>
			<th>TLS 1.2</th>	
            <th>SHA1</th>
            <th>RC4</th>
            <th>Forward Secrecy</th>
            <th>POODL</th>
        </tr>
    </thead>
    <tbody >
    	<tr>
        	<td>Bank of Montreal</td>
            <td>B</td>
            <td>Pass</td>
            <td>Pass</td>
            <td>Pass</td>
            <td>Fail</td>
            <td>Fail</td>
            <td>Pass</td>
        </tr>
    	<tr>
        	<td>CIBC</td>
            <td>B</td>
            <td>Pass</td>
            <td>Pass</td>
            <td>Pass</td>
            <td>Fail</td>
            <td>Fail</td>
            <td>Pass</td>
        </tr>
    	<tr>
        	<td>Royal Bank</td>
            <td>B</td>
            <td>Pass</td>
            <td>Pass</td>
            <td>Pass</td>
            <td>Fail</td>
            <td>Fail</td>
            <td>Pass</td>
        </tr>
        <tr>
        	<td>Scotia Bank</td>
            <td>B</td>
            <td>Pass</td>
            <td>Pass</td>
            <td>Pass</td>
            <td>Fail</td>
            <td>Fail</td>
            <td>Pass</td>
        </tr>
        <tr>
        	<td>Toronto Dominion</td>
            <td>B</td>
            <td>Pass</td>
            <td>Pass</td>
            <td>Pass</td>
            <td>Fail</td>
            <td>Fail</td>
            <td>Pass</td>
        </tr>
    </tbody>
</table>

So everybody is running with a grade of B and everybody is restricted to B because they still accept the RC4 cypher. There are some attacks available on RC4 but they don't currently appear to be practical. That's not to say that they won't become practical in short order. The banks should certainly be leading the charge against RC4 because it is possible that when a practical exploit is found that it will be found by somebody who won't be honest enough to report it.

Out of curiousity I tried the same test on some of Canada's smaller banks such as ATB Financial(I have a friend who works there an I really wanted to stick it to him for having bad security).

<table style="font-size:12px">
	<thead>
    	<tr>
        	<th>Bank</th>
            <th>Grade</th>
            <th>SSL 3</th>
			<th>TLS 1.2</th>	
            <th>SHA1</th>
            <th>RC4</th>
            <th>Forward Secrecy</th>
            <th>POODL</th>
        </tr>
    </thead>
    <tbody>
    	<tr>
        	<td>ATB Financial</td>
            <td>A</td>
            <td>Pass</td>
            <td>Pass</td>
            <td>Pass</td>
            <td>Pass</td>
            <td>Pass</td>
            <td>Pass</td>
        </tr>
        <tr>
        	<td>Banque Laurentienne</td>
            <td>A</td>
            <td>Pass</td>
            <td>Pass</td>
            <td>Pass</td>
            <td>Pass</td>
            <td>Pass</td>
            <td>Pass</td>
        </tr>
        <tr>
        	<td>Canadian Western Bank</td>
            <td>A</td>
            <td>Pass</td>
            <td>Pass</td>
            <td>Pass</td>
            <td>Pass</td>
            <td>Pass</td>
            <td>Pass</td>
        </tr>
    </tbody>
</table>

So all these little banks are doing a really good job, that's nice to see. It is a shame they can't get their big banking friends to fix their stuff. 

##But Simon, we have to support old browsers

Remember that time that your doctor suggested that your fluids were out of balance and you needed to be bled? No? That's because we've moved on. For most things I recommend looking at your user statistics to see what percentage of your users you're risking alienating if you use a feature that isn't in their browser. I cannot recommend the same approach when dealing with security. This is one area where requiring newer browsers is a good call - allowing your users to be under the false impression that their connection is secure is a great disservice. 
