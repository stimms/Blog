---
layout: post
title: There is No Feature Flag Conspiracy
authorId: simon_timms
date: 2013-02-11
---

I know what they say, that if there were a conspiracy we would deny its existence but honestly those of us who support feature flags didn't even know that we were suppose to be conspiring. In either case one of those smart guys I've talked about in the past, Amir Barylko, just wrote a [blog post](http://orthocoders.com/blog/2013/02/03/the-attack-of-the-killer-feature-branch/) in which he derides the idea of feature flags and supports branches instead.

Thedefinition which Amir givesof feature flags doesn't sitentirelywell with me. A feature flag is a branch in the flow of the application which disables a chunk of functionality for some or all users. These flow branches can be either compile time switches using ifdefs, if statements in the code or even a strategy pattern where different strategies are plugged in.

Let's take a step back for a moment and talk about why we branch our code. I think there are a number of reasons for and applications of branching.

1. You want to develop a feature which will take some time and will not beimmediatelyvisible to users. So perhaps you want to add a function which e-mails users when some long running action they have started completes. This will take more than the 3 hours you have left in the day so you branch and, when the feature is complete, you merge it back into trunk and all is well. These feature branches may live for hours, days, weeks or even months and may be worked upon by several people.

2. You're supporting multiple versions of the application. I know this is hard to remember in the reality of software as a service but there was once a time when you would deliver a product and then supply updates and patches for a time while having already started developing the next version. You don't want users of the previous version to get the full update to the next version without paying but at the same time you don't want them to suffer a bug which is fixed in the next version. In this case you maintain a branch for each release in which you can add fixes.

3. When you're working on a feature and something more important comes along so you shove your work in a branch and come back to it later.

4. Quality branches when you promote features from dev to test to production. This ensures that only features which are ready for production make it there and they make it through test.

5. To support multiple parallel developments against different technologies. Perhaps your product has a version for Windows and a version for UNIX. Between the two of them there likely exists a shared kernel but you keep different branches for each platform and merge into them from the shared kernel.

Huh, I was sort of expecting that to be a longer list. I may have missed something in here but I don't think so. I don't believe that either branching or feature flags is the magic pill which solves each one of these use cases optimally.

Amir's argument boils down to: git is really good at merging and feature flags add a lot of overhead without offering anything that branching doesn't. Well let's look at each of the cases above and see if that holds true.


# Feature Branches

Feature branches keep new functionality isolated from users until it is ready. I don't like the idea of branching for features right off the bat. It gives the idea that the feature is a bigmonolithicpiece of code which needs to be completed before it is ready for users. Why is that dangerous? Because developing functionality in isolation leads to functionality which users don't really want or don't work in the way the users envisioned. Feature flags are a clear winner here in my mind. Feature flags allow developers to turn features on at runtime to test out new functionality on small sets of users or try them out at certain times of day.

In many large systems the testing environment isdifferentenough form production that it is almost impossible to know what will happen when a feature is turned on. The production environment can be too expensive to reproduce or too diverse. In these cases you turn the features on for a few people and see what happens. It sounds crazy but there arelegitimatelysome things you can't test outside of production. There are also some things you want to test in production. If you're scientific about making changes to your application then you want to to AB testing against a real user base.

Branching for features delays integration with mainline. It means that you'repurposefullyadding uncertainty about what will happen when the feature is merged back in. In the comments to another post on this subject, [Dyaln Smith's](http://geekswithblogs.net/Optikal/archive/2013/02/10/152069.aspx),Amir suggests that the delayed integration problem is a myth. Each feature branch is responsible for merging from mainline, he claims, so in fact integration is happening all the time. That's not true. Integration is happening between mainline and each feature branch but not between different feature branches.

If features are very small and short lived then branching remains an option. In my mind if the development of a feature is expected to take more than 24 hours then a branch is not the right approach.


# Branching to SupportMultipleReleases

I think this is a totally legitimate use of branching and a terrible place to use feature flags. The reason here is that the changes to the maintenance branch are always enabled. You would never want to put in a change to maintenance branch which wouldn't be enabled right away. Source control systems like git are magnificent at merging between code bases. I remember merging between branches in ClearCase and in Subversion. These legacy source control systems were not good at this uses case â€“ it was almost always easier to just manually make the changes in two places. It is really difficult to scale beyond two releases. When I was working with ClearCase we supported about 10simultaneousreleases. It was not fun.


# Branching to Shelve Work

Again legitimate. When something more important comes along you may not have time to finish the line on which you're working so the code base could not compile. Committing this back into mainline would bedisastrous (for a pretty benign definition of disaster, I admit). These branches are fine in my mind because they're really short lived. Remember that: branchesshouldbe short lived.


# Quality Branches

I don't really think these differ much from feature branches. It is just a mechanism for promoting code between environments. Feature flags are a better choice here as they give better granularity for promotion and avoid the nightmare that is trying to isolate, at merge time what needs to be promoted. Merging upstream is a difficult problem. I've seen it used before in environments which strongly regulate what goes into production. We had terrible issues with merging and breaking upstream code. It was especiallyproblematicas this was many years ago before automated testing had really caught on. We did a lot of â€œemergencyâ€ fixes.


# Supporting Multi Environment Development

Yuck. This is a totallyillegitimateplace to use branching. Different code for different environments should be isolated into different projects in mainline. If you create a branch for each one you're buying a huge number of branches. Remember when I said I worked at a place where we supported 10releases? We also supported 31 platforms. 31. Heck we supported 4 different builds on Solaris: x86, x86_64, sparc and sparc64. Then we brought on a new compiler for Solaris which created binaries which were not backwards compatible. That added 3 more platforms, just for Solaris. I cannot even imagine how much time would have been wasted merging between branches if we had a different branch for each. It would have taken 90% of our development cycle just to push stuff to the various branches.


# Arguments Against Feature Flags

A common argument I hear is that feature flags need to be toggled in a lot of places. If a change happens in the UI and the back end then that's at least 2 places you need to put in the flag. This is a reasonable concern. The beauty of feature flags is that once the feature is fully live then the flags can be removed. Centralizing feature flag implementations is also key to implementing feature flags. Testing feature flags should be isolated to a single piece of code. I would also recommend leaning on the compiler a bit for feature flags. Instead of using strings to denote the name of a flag use an enum. In this way it is easy to find all references to the value and quickly jump to the feature flags used in code.

I really like feature flags for the flexibility they offer me for when I deploy features and for whom they are turned on. They're not good in every situation and, as with all things, you have to think before you use them. There is a very realpossibilitythat feature flags will grow out of control and turn your code into spagetti. These concerns should be addressed the same way you would address any code quality concern: with thought and refactoring.



