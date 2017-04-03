---
layout: post
title: The ONE API
authorId: simon_timms
date: 2011-01-25
---

My boss was pretty excited when he came back from some sort of mobile technology symposium offered in Calgary last week. I like it when he goes to these things because he comes back full of wild ideas and I get to implement them or at least I get to think about implementing them which is almost as good. In this case it was **THE ONE API**. I kid you not, this is actually the name of the thing. It is an API for sending text messages to mobile phone users, getting their location and charging them money. We were interested in sending SMS messages, not because we're a vertical marketing firm or have some fantastic news for you about your recent win of 2000 travel dollars, we have legitimate reasons. Mostly.

Anyway I signed up for an account over at [https://canada.oneapi.gsmworld.com/](https://canada.oneapi.gsmworld.com/) and waited for them to authorize my account. I guess the company is out of the UK so it took them until office hours in GMT to get me the account. No big deal. In I logged and headed over to the API documentation. They offer a SOAP and a REST version of the API so obviously I fired up curl and headed over to the sandbox url documentation in hand. It didn't work. Not at all.

<div style="padding-left:10px;background-color:#d3d3d3;">curl https://canada.oneapi.gsmworld.com/SendSmsService/rest/sandbox/ -d version=0.91 -d address=tel:+14034111111 -d message=APITEST </div>In theory this command should have sent me a message(I changed the phone number so you internet jerks can't actually call me) or at worst return a helpful error message, so said the API documentation.

What actually happened was that it failed with a general 501 error, wrapped in XML. Not good, the error should be in JSON, so says the API docs. It also shouldn't fail and if it does the error should be specific enough for me to fix. I traced the request and I was sending exactly what I should have been.

No big deal, I'll try the SOAP API and then take my dinosaur for a walk. The WSDL they provided contained links to other WSDLs, a pretty common practice. However the URLs in the WSDL were pointing to some machine which was clearly behind their firewall making it impossible for me to use it.

I gave up at that point. These guys are not the only people who are competing in the SMS space and if they can't get the simplest part of their service, the API, right I think we're done here. Add to this that they only support the three major telcos in Canada(Telus, Bell and Rogers) and there are much better options available. [Twilio](http://www.twilio.com) supports all carriers in Canada and the US and they charge, at current exchange rates, 1.5 cents a message less than these guys. Sorry **THE ONE API** you've been replaced by a better API, cheaper messaging and better compatibility.



