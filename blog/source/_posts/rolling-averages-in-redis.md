---
layout: post
title: Rolling Averages in Redis
authorId: simon_timms
date: 2014-08-13
---

I'm working on a system that consumes a bunch of readings from a sensor and I thought how nice it would be if I could get a rolling average into Redis. As I'm going to be consuming quite a few of these pieces of data I'd rather not fetch the current value from redis, add to it and send it back. I was thinking about just storing the aggregate value and a counter in a hash set in Redis and then dividing one by the other when I needed the true value. You can set multiple hash values at the same time with HMSET and you can increment a value using a float inside a hash using HINCRBYFLOAT but there is no way to combine the two. I was complaining that there is noHMINCRBYFLOAT command in Redis on twitter when Itamar Haber suggested writing my own in Lua.

I did not know it but apparently you can write your own functions that plug into Redis and can become commands. Nifty! I managed to dig up a quick Lua tutorial that listed the syntax and I got to work. Instead of aHMINCRBYFLOAT function I thought I could just shift the entire rolling average into Redis.

<script src='https://gist.github.com/stimms/8ff3c43caf82ac3978a8.js'></script>

This script gets the current value of the field as well as a counter of the number of records that have been entered into this field. By convention I'm calling this counter *key*.count. I increment the counter and use it to weight the old and new values.

I'm using the excellent StackExchange.Redis client so to test out my function I created a test that looked like

<script src='https://gist.github.com/stimms/846a8107e451c2afbcf7.js'></script>

The test passed perfectly even against the Redis hosted on Azure. This script saves me a database trip for every value I take in from the sensors. On an average day this could reduce the number of requests to Redis by a couple of million.

The script is a bit large to transmit to the server each time. However if you take the SHA1digest of the script and pass that in instead then Redis will use the cached version of the script that matches the given SHA1. You can calculate the SHA1 like so

<script src='https://gist.github.com/stimms/9e7681d3fc3783205947.js'></script>

The full test looks like

<script src='https://gist.github.com/stimms/a6bcdeb9248b7f85f5ed.js'></script>



