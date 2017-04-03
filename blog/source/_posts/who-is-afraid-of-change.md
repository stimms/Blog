---
layout: post
title: Who is Afraid of Change?
authorId: simon_timms
date: 2013-01-17
---

If you've ever worked for a large company then you've probably run into a change management policy. If you've been particularly bad in a previous life and I'm talking tremendously bad, maybe you caused the extinction of a species or you failed to equip the Titanic with enough life boats or you composed country music, then you have had to deal with a change control board. Just the words strike fear into my heart. The only way it could be worse is if you slap the word â€œglobalâ€ or â€œenterpriseâ€ into the phrase:

Global Enterprise Change Management Board

When I heard the phrase I use to picture a mystical circle of elders who, filled with the knowledge of decades of experience and probably a dozen degrees a piece. They would have such integral knowledge of the systems that they would just be able to look at a requested change and intuit what would break.

<div class="wp-caption alignnone" id="attachment_2123" style="width: 310px">[![The Change Management Board](http://stimms.files.wordpress.com/2013/01/grycncl.jpg?w=300)](http://stimms.files.wordpress.com/2013/01/grycncl.jpg)The Change Management Board

</div>I don't think that anymore. The truth is that change management boards tend to be populated with management types who really have no idea how things actually work. Their technical knowledge is out of date and they have to rely on the people requesting the change for background knowledge. If you're looking to the person requesting the change to describe and analyse the change then why even bother having a change management board? No reasonable checks andbalancesare provided.

The purpose of a change management process is to try to restrict unexpected side effects from changes to the computing environment in a company. What it actually does is slow down change and make IT less responsive to the changing business environment. If the effects of a change aren't known then why are you making the change? You're basically saying â€œwe don't understand our environment sufficiently well to know what we're doingâ€.

Many would argue that this is understandable, after all the computing environment at large companies is stunningly complicated. I don't buy it. If you go to your doctor they understand the body well enough that they know what the side effects could be of giving medicines. Through experimentation and analysis medical science has cataloged the possible side effects of using medicines. We would not accept doctors giving us medicines which aren't understood so why are we accept the same thing from our IT groups? The human body is many many orders of magnitude more complicated than even the most complex enterprise networks.

We are fortunate to live in a time of virtual machines and provisioning tools like [Chef](http://www.opscode.com/chef/) and [Puppet](http://puppetlabs.com/). Using these tools it is possible to easily establish full test environment in a matter of minutes. The test environment can be a scaled down version of the production environment. This gives an amazing tool for testing any change. Even if your company doesn't have the spare computers to provision an environment then you can still build a test environment on EC2.

If you're afraid of change and feel the need to try to control it using a change management board or a rigid change management process then you would be far better off spending your time developing a reproducible testing environment and a suite of tests. Frankly not having a solid testing environment is irresponsible. No amount of change management is better than a realistic testing regim. This isn't a problem of lack of regulation, it is a problem of lack of understanding and professionalism. It is a cognitivedissonanceof restricting change when the business is based on change. It has got to be fixed.



