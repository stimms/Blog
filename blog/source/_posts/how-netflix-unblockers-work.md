---
layout: post
title: How Netflix Unblockers Work
authorId: simon_timms
date: 2013-05-24
---

Note: I'm totally guessing at how a lot of this stuff works so maybe don't use it as the basis of building a space ship or anything important.

Here in Canada we get Netflix but it isn't the same as the US Netflix. If it wasn't for the fact that Canadian Netflix has Startgate SG-1 and US doesn't I would be much angrier about the whole thing. None the less there are times when I want to watch something which isn't available in Canada. I was always aware that there were services and ways of watching US Netflix in Canada but I hadn't really bothered looking into until a friend of mine mentioned Blockless. Their service is really simple, you just need to change the DNS settings on your router and away you go. This was way easier than the SSH backed VPN tunnel to an Amazon virtual machine I had envisioned.

<div class="wp-caption aligncenter" style="width: 346px">![](https://www.blockless.com/img/bl.logo.png)Blockless use cool DNS tricks to get you Netflix

</div>Well that got me thinking, how the heck does this work? Well if I was going to set up a geographically specific service I would check theincomingIP addresses of connections and then block or allow them as I saw fit or serve out different content. This doesn't, however, seem to be the way that Netflix works. Instead they seem to use a technology called GeoDNS. This is the same technology which is behind CDNs which deliver content from the nearest node.

When you ask the DNS server to give you an IP address for a name it usually just blindly hands you back an address. With a GeoDNS service the server will first look up your location and then return the IP address of the node which is closest. Netflix uses this to ensure that if you're in Canada you get Canadian content.

When you change your DNS server with Blockless or Unblock Us they intercept the DNS request and substitue the US location in for the Canadian IP address. With that you now get US Netflix. The same thing can be done to simulate being in any country you want.

This is a really great solution to the pain of geolocking, but it only works if the service you're talking to take a naive approach to their filtering. IF Netflix also checked the incomming IP address when they were serving the content then you would need to go with a fully fledged VPN. With a VPN the actual end point accessing Netflix would be in the US and there would be no real way for Netflix to filter it.



