---
layout: post
title: Using Realistic Data in Unit Testing
authorId: simon_timms
date: 2013-02-24
---

I was writing some unit tests the other day around some code what was missing them and I encountered a couple of field names which confused me. They were well named fields but within the domain I was working they could have a couple of different meanings. This isn't the fault of the programmer and there are times when even the domain experts are a bit muddled about their terminology. Ideally we developers should work with the domain experts to sort out their language and ensure that everybody has a solid understanding of the domain. This isn't always possible and you might end up with a field like PlantName which could be a couple of things.

I wasn't keen on changing the field name as the code interfaced with other systems and I really didn't want to coordinatesimultaneousreleases with our IT department. What I did, instead, was use the unit tests to clarify the content of the fields. The two meanings of theambiguouslynamed field had very different looking data in them. So I made sure that the data I put in corresponded with the right meaning for the field.

This isn't what I typically do. I constantly use test data which is a random splat of letters or numbers without meaning. If, however, you take the extra few seconds needed to put in realistic data then you gain a couple of advantages:

1. <span style="line-height:13px;">Thosereading your testsimmediatelygain a deeper understanding of what it is you're testing.</span>
2. You may uncover subtile bugs which you would miss with random data. For instance consider an application which did word stemming: you're likely to see a different result if you use the test data "running"(run) instead of "fkjdsklfjadsli".

While I was thinking about this Istumbledon the blog of my Calgary .net hero David Paquette. Dave, in conjunction with James Chambers, [announced tool called AngelaSmith ](http://www.davepaquette.com/archive/2013/01/14/go-beyond-lorem-ipsum-with-angelasmith.aspx)named after the British Labour Party Politician who was instrumental in changing the law to afford those attacked by dogs better protections. Dave and James are real activists for the protection of those injured in dog attacks so I can see why they are using the name. Their tool allows for the creation of realistic test data using a variable name based strategy.

What's that mean? It means that if you have common names in your code like "FirstName" or "LastName" then AngelaSmith will put in realistic values from its own database. AngelaSmith has built in values for a lot of common fields but if you have something weird, like I did you can put in a custom population strategy and have AngelaSmith generate values for you.

The project is pretty much brand new but it is a great idea and I hope it is something which James and Dave choose to continue.

I'll post up some examples of how to use and extend AngelaSmith in a bit but for now the introduction on the github page is sufficient:https://github.com/MisterJames/AngelaSmith



