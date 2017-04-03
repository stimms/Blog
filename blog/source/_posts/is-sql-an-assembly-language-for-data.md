---
layout: post
title: Is SQL an Assembly Language for Data?
authorId: simon_timms
date: 2013-06-12
---

I talk a lot about alternatives to writing JavaScript(typescript, coffeescript, dart,"¦) and that JavaScript is really just an assembly language for the web. That got me thinking about SQL and whether we should be considering it an assembly language for databases.

SQL has a lot of problems which make it more difficult to use. Just look at the [list of key words for it](http://msdn.microsoft.com/en-us/library/aa238507(v=sql.80).aspx): the list is huge and the list of future reserved words bring the list to crazy levels. It would be nicer if the keywords were moved from being keywords to being library functions. The syntax is also not conducive to providing autocompletes. Consider the simple SQL

select Id, Name, Number from tblThingies

The autocomplete domain is the set of columns in tblThingies, because this is naturally typed from left to right (at least in English, how does this work in Arabic?) the domain isn't defined until after the terms Id, Name and Number are written. This is not at all conducive to autocomplete.

The concepts in SQL are sound. Largely functions in SQL are set operations and if you treat the language as a set manipulation language instead of a procedural language then it is really good at it. Modern languages have a different approach to syntax and formatting from which SQL couldbenefit.

There are actually a number of languages which do compile to SQL. LINQ is one example and the HQL language from Hibernate and NHibernate is another. I think HQL gives a good example of what a more modern query language looks like. From the HQL community documentation I took this example:

<script src='https://gist.github.com/stimms/5771170.js'></script>

You can see the select ordering still doesn't provide us autocomplete capabilities and the key words are still very SQLly. I don't think that HQL goes far enough in fixing the problems of SQL LINQ does a better job. That same query in LINQ would look something like

<script src='https://gist.github.com/stimms/5771209.js'></script>

(although I haven't tested this).

You can see the select is moved to the end and at the same time the select is transfigured into a more object oriented syntax. I like the idea of returning objects by default. It makes the language more powerful and it reduces the friction in talking to OO languages which are going to be the majority of languages now.

I don't think we've explored the idea of SQL as an assembly language enough yet. LINQ is available through LINQ pad but I'm not aware of any standalone system which allows for HQL queries to be run against a server. I like the idea of compiling languages to SQL. The syntax is going to take some work but it would be great to see something adopted by the major vendors.



