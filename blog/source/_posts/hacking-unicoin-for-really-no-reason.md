---
layout: post
title: Hacking Unicoin for Really no Reason
authorId: simon_timms
date: 2014-04-01
---

It is April 1st today which means that all manner of tom-foolery is afoot. Apart from WestJet's brilliant "[metric time](https://www.youtube.com/watch?v=DcbO3hl3_F0)" joke the best one I've seen today is Stack Overflow's introduction of Unicoin which is a form of digital currency which can be used to purchase special effects on their site.

[![unicoin](http://stimms.files.wordpress.com/2014/04/unicoin.png)](http://stimms.files.wordpress.com/2014/04/unicoin.png)

To get Unicoin you have two options: buy it or mine it. I have no idea if buying it actually works and at $9.99 for 100 coins I'm not going to experiment to see if you can actually purchase it. Mining it involves playing a fun little game where you have to click on rocks to uncover what they have under them(could be coins, could be nothing).

[![unicoin2](http://stimms.files.wordpress.com/2014/04/unicoin2.png)](http://stimms.files.wordpress.com/2014/04/unicoin2.png)

I played for a few minutes but got quickly tired of clicking. I'm old and clicking takes a toll. To unlock all the prizes you need to have about 800 coins (799 to be exact). So I fired up the F12 developer tools to see if I could figure out how the thing was working.

As it turns out there are two phases to showing and scoring a rock. The first one is rock retrieval which is accomplished by a GET tohttp://stackoverflow.com/unicoin/rock?_=1396372372225 or similar. That parameter looked familiar to me and, indeed, it is a timestamp. This will return a new "rock" which is just JSON

{"rock":"DAUezpi1zrfxHRxdi3yp9JUCZ9vwABJbDA"}

The value appears to be some sort of randomly generated value. Doesn't really matter for our purposes. The response once a rock is mined is to POST to

http://stackoverflow.com/unicoin/mine?rock=DAUezpi1zrfxHRxdi3yp9JUCZ9vwABJbDA

in the body of that POST you'll need an fkey which can be found by looking at the value in StackExchange.options.user.fkey.

Once you know that stealing coin is as easy as

<script src='https://gist.github.com/stimms/9919085.js'></script>

There appears to be some throttling behaviour built in so I ran my requests every 15 seconds. Hilariously if you don't include the fkey the server will respond with HTTP 418, an April Fools inside and April Fools. Now you can buy whatever powerups you want

[![unicoin3](http://stimms.files.wordpress.com/2014/04/unicoin3.png)](http://stimms.files.wordpress.com/2014/04/unicoin3.png)



**Update: **The rate at which I'm discovering new Unicoins has dropped off rapidly. I was discovering coins on almost every hit originally now it is perhaps 1:20. Either I'm being throttled or the rate of discovery of new coins reduces as more of the keyspace has been explored like Bitcoin. I really hope it is the second one, that would be super nifty.



