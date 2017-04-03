---
layout: post
title: Creating DataBars in EPPlus
authorId: simon_timms
date: 2013-03-06
---

Irritatingly frequently I encounter a request which basically boils down to â€œI have this amazingly complicated excel spreadsheet I'm using to stop the company from going bankrupt, can you make yourapplicationproduce it?â€. So I bravely dive into it and find 96 tabs and graphs and the what not. I am terrified that so much of the business world relies on spreadsheets of this sort but my nightmarish fears are not the subject of this blog. Well not today.

I make use of theabsolutelyfantastic excel library [EPPlus](http://epplus.codeplex.com/). In today's nightmare I was to create a databar. This is a special sort of conditional formatting which basically creates a bar inside a cell.[![Screen Shot 2013-03-06 at 8.52.25 PM](http://stimms.files.wordpress.com/2013/03/screen-shot-2013-03-06-at-8-52-25-pm.jpg)](http://stimms.files.wordpress.com/2013/03/screen-shot-2013-03-06-at-8-52-25-pm.jpg)

The length of the bar is calculated based on the range between the largest and smallest values in the range. Aren't they pretty?

The latest version of EPPlus adds support for conditional formatting and you too can create these sorts of spreadsheets programatically.

<script src='https://gist.github.com/stimms/5105497.js'></script>

Basically all you do is select the range you want and assign the conditional formatting of type databar to it. As far as I can tell there is no support yet for solid fill bars.



