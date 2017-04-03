---
layout: post
title: Automatic Credit Card Type Selection
authorId: simon_timms
date: 2013-06-14
---

I can't tell you the number of times I go to a website to buy something and I have to select the type of credit card being used. It is a minor step but by gum you want to make checking out of your website as painless as possible for people. That's why Amazon's one click was such an amazing idea. I frequently get to that last step and abandon the order because I've thought better of it (sorry, [WebStorm](http://www.jetbrains.com/webstorm/), one of these days).

Instead of asking for a credit card type why not use these simple rules(Taken from [eHow](http://www.ehow.com/list_7172242_credit-card-validation-rules.html))

The first two to six digits of a credit card determine who the credit provider is. For example, Master Card begins with the numbers 51-55, American Express begins with 34 or 37 and Discover begins with 6011

So it looks like from the first few digits you can tell what the card type is without having to ask your users. There is a much more complete list of the prefixes on [Wikipedia](http://en.wikipedia.org/wiki/Bank_card_number#Issuer_identification_number_.28IIN.29). According to that article these card numbers are actually part of a larger scheme which can identify the major industry as well as the sort of card.

As an example of detecting the card type I threw together a little script. You can try it out [here](http://bl.ocks.org/stimms/5786201). It currently detects Visa, MasterCard, American Express and China Union cards(hey, they have 1.2 million ATMs, they're doing something right). Just type in a couple of digits.

- 4 Visa
- 34 American Express
- "¦

So much better than making people type and so quickly done.



