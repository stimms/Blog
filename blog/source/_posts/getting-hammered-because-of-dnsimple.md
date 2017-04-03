---
layout: post
title: Getting Hammered Because of DNSimple?
authorId: simon_timms
date: 2014-12-01
---

Poor DNSimple is, as of writing, undergoing a massive denial of service attack. I have a number of domains with them and, up until now, I've been very happy with them. Now it isn't fair of me to blame them for my misfortune as I should have put in place a redundant DNS server. I've never seen a DNS system go belly up in this fashion before. I also keep the TTL on my DNS records pretty low to mitigate any failures on my hosting provider. This means that when the DNS system fails people's caches are emptied very quickly.

DNS has been up and down all day but it is so bad now that something had to be done. Obviously I need to have some redundancy anyway so I set up an account on [easyDNS](https://www.easydns.com/). I chose them because their logo contains a lightning bolt which is yellow and yellow rhymes with mellow and that reminds me of my co-worker, Lyndsay, who is super calm about everything and never gets upset. It probably doesn't matter much which DNS provider you use so long as it isn't Big Bob's Discount DNS.

I set up a new account in there and put in the same DNS information I'd managed to retrieve from DNSimple during one of its brief periods of being up. I had the information written down too so either way it wouldn't be too serious to recreate it. It does suggest, however, that there is something else you need to backup.

In EasyDNS I set up a new domain

[![Screen Shot 2014-12-01 at 10.11.29 PM](https://stimms.files.wordpress.com/2014/12/screen-shot-2014-12-01-at-10-11-29-pm.jpg)](https://stimms.files.wordpress.com/2014/12/screen-shot-2014-12-01-at-10-11-29-pm.jpg)

in the DNS section I set up the same records as I had in my DNSimple account.[![Screen Shot 2014-12-01 at 10.14.02 PM](https://stimms.files.wordpress.com/2014/12/screen-shot-2014-12-01-at-10-14-02-pm.jpg)](https://stimms.files.wordpress.com/2014/12/screen-shot-2014-12-01-at-10-14-02-pm.jpg)Finally I jumped over to my registrar and entered two of the EasyDNS server as the DNS servers for my domain and left two DNSimple servers. This is not the ideal way of setting up multiple DNS server. However from what I can tell DNSimple doesn't support zone transfers or secondary DNS so the round robin approach is as good as I'm going to get.

[![Screen Shot 2014-12-01 at 10.34.34 PM](https://stimms.files.wordpress.com/2014/12/screen-shot-2014-12-01-at-10-34-34-pm.jpg)](https://stimms.files.wordpress.com/2014/12/screen-shot-2014-12-01-at-10-34-34-pm.jpg)

With the new records in place and the registrar changed over everything started working much better. So now I have redundant DNS servers for about $20/year. Great deal.



