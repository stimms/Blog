---
layout: post
title: Missing Constraints in ParkPlus
authorId: simon_timms
date: 2015-01-09
---
Parking in Calgary is always a bit of an adventure, as it is in most North American cities. All the city owned parking and even some private lots are managed by a company called The Calgary Parking Authority a name that is wholly reminiscent of some Orwellian nightmare. Any organization that gives out parking tickets is certain to be universally hated so I feel for anybody who works there. 

What sets the Calgary Parking Authority apart is that they have developed this rather nifty technology to police the parking. They call it park plus and it comes in a couple of parts. The first is a series of signs around town with unique numbers on them. 

![Zone sign](http://i.imgur.com/3rFA72a.jpg)

These identify the parking zone you're in. A zone might span a single city block or it might cover a large multi-storey parking lot. The next is the parking terminal or kiosk

![Parkplus kiosk](http://i.imgur.com/7ietZge.jpg)

These solar-powered boxes act as data entry terminals. They are scattered around the city with one a block in most places. In to them you enter your license plate number and the zone number along with some form of payment. 

The final piece of the puzzle is a fleet of camera equipped vehicles that drive up and down the streets taking pictures of license plates. 

![Camera car](http://i.imgur.com/uKLcrev.jpg)

If the license plate is in a zone for which payment is required and no payment has been made then they mail you out a ticket. Other than driving the vehicles there is almost no human involvement on their side. It is actually a really good application of technology to solve a problem. 

Unless something goes wrong, that is. 

I used one of these things a few weeks ago and it told me that my form of payment was unrecognized. The error seemed odd to me as VISA is pretty well known. So I tried MasterCard, same deal. American Express? Same deal. Dinner's club? Haha, who has one of those? "This kiosk", I thought, "is dead". Now the customer friendly approach here is the Calgary Parking Authority to admit that if a kiosk is broken you don't have to pay. So of course they do the opposite and make you walk to another kiosk. In my case 5 other kiosks.
Every kiosk told me the same thing. Eventually I gave up and fed it cash. 

A few weeks pass and I get my VISA statement. Lo and behold there are multiple charges for Calgary parking for the same time period, for the same license plate in the same zone. This is a condition that should not be permitted. When charging the credit card it is trivial to check to database to see if there is already a parking window that would cover the new transaction. If it exists and the new window is not substantially longer than the current window then the charge should not be applied. 

I'm disappointed that there is such a simple technical solution here that isn't being applied.  

I have another whole rant about the difficulty of getting my money refunded but you wouldn't be interested in that.
