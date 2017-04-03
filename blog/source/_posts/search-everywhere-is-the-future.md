---
layout: post
title: Search Everywhere is the Future
authorId: simon_timms
date: 2013-02-27
---

Today I was demoed an in house application. It was what I have come to think of as a typical in house application: that is say lacking some attention to usability. I see this all the time and I think that not spending time on good usability and attractive design serves to make people think less of the application. You hear "Oh that accounting software sucks, it is so hard to use" and eventually the software is replaced sooner than needed and at a higher cost. Anyway that's not what this blog is about, it is about search and how it is the killer user interface.

In the application I was looking at, let's give it a random 3 letter acronym as all with in house software: TYM, there was a form which required you to select an employee. The employee selection was a drop down box with 2600 entries in it. I asked why the usability on that drop down was so bad. Why weren't there at least some filters? "Oh," said the lady demoing it ,"you can use hyperscroll". Hyperscroll?! I haven't seen hyperscroll before so I had her demo it. Turns out it is just the search you get when you go into a drop down and type the first letters of the entry. In a list of 2600 entries that approach isn't great. There is limited space in a drop down and the UI paradigm just doesn't adapt well to that many entries. It also isn't good if you're looking for a last name and the list is sorted by first name.

This is a perfect place to use a search. Search doesn't have to be big and involved it can be very simple and small, it can even be an autocomplete box. Heck, you don't even need to make the search server side! I believe that in an era of lots of data the utility of the drop down, or combo box is limited. There are still some times when the entries in a drop down are fewer than, say, 10 that a drop down is the correct approach. Of course it is difficult to know when your data will exceed the number of entries so you might be better off building the search in right off the bat.

This wasn't the only place in TYM which would havebenefitedfrom a search. There was another form which contained a wall of check boxes. I mocked up this example but TYM was far more extreme.

<div class="wp-caption aligncenter" id="attachment_2376" style="width: 310px">[![Checkboxtopia](http://stimms.files.wordpress.com/2013/02/screen-shot-2013-02-26-at-7-58-27-pm.jpg?w=300)](http://stimms.files.wordpress.com/2013/02/screen-shot-2013-02-26-at-7-58-27-pm.jpg)Checkboxtopia

</div>Again this is a place where search would have been a better solution. The typical approach I take in this situation is to provide a tagging mechanism like the one provided by [TagIt](http://webspirited.com/tagit/). This approach it not asintuitiveto users but once they have the hang of it they will like it far more.

I really think that a few minutes of care and attention in user interface design will win your users over to you.



