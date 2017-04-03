---
layout: post
title: On Hiding Data
authorId: simon_timms
date: 2013-04-01
---

I had a veryinterestingissuesubmitted over the weekend; one of several which boiled down to the same issue. The application against which it was submitted has a number of categories. Some of the categories are empty, which is just fine in the application. We made the mistake of underestimating our users and we started to hide empty categories. We assumed that when filtering users wouldn't want to see categories which would give them empty results. Why would you want to see empty categories? Well as it turns out there are a ton of uses cases:

- When adding a new category it will be empty by default and users would like to see that their addition of a category worked
- When generating reports users want to have proof that the category's contents are missing
- When listing categories users have an expectation that none will be missed

We hid the categories to reduce options in a drop down and to generally clean up the UI. Now I've had a number of issues on this topic submitted by users we're going to remove the empty category filters. We're actually hurting our users by trying to guess at what they want instead of asking them and letting them try things out. Lessons learned in software development, I suppose.



