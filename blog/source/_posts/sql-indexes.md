---
layout: post
title: SQL Indexes
authorId: simon_timms
date: 2013-05-16
---

Final talk dump from PrDC13! This puppy is about SQL indexes with the always scary smart [Michael DeFehr](https://twitter.com/mdefehr). I'm so terrible at

- <span style="line-height:13px;">SQL server uses 8K pages which are organized in a tree structure</span>
- By the time we get to 3 levels deep there are enough pages that almost all tables fit in that space
- Each level of the index is also a doubly-linked list which allows transversal toneighbourpages
- An index is made up of 3 groups of columns: - key column
- columns in tree
- columns in leaf
- nonclustered index has the clustering key in the leaf and will have it in the tree if not a unique index
- This will give detailed information about the breakdown of the index statistics, numbers of levels and the such

select * from sys.dm_db_index_physical_stats(DB_id(), Object_id('tablename'), 1,null,'DETAILED') ddips

- You can set statistics on by setting

SET STATISTICS IO ON

- Index seek is just a traversal though the index using a binary search
- If the data in an index is properly sorted the you can pull out chunks without having to worry about running a sort operation. This makes the fields indexed super important
- Using keyboard options in the management studio allows you to insert the highlighted portion as an option to some code so you can put "select * from" in the shortcuts then you can highlight a table name in the editor and hit the shortcut to select * from that table
- Download and use [this stored proc ](http://www.sqlskills.com/blogs/kimberly/use-this-sp_helpindex-rewrites/)for analysing the indexes
- Indexes are always unique, even when they aren't. Behind the scenes the index will add the primary key into the tree to ensure uniqueness if you don'tspecifyunique
- Included columns in an index include the value from that column in the index page. So if your index is on FishId and you include FishName then when you query a non-clusteredindex for just FishId and FishName then there is no reason to hit the table itself. As soon as you ask for something more than that you have to go to the table itself which involves a seek and then a lookup.
- You typically don't need all the columns in a table. Using projections is a good improvement.
- Filtered indexes allow for indexing only part of a table. Say you have an append only table and you only want to query on the last day of data then you can restrict the index to only contain rows which match some criteria. This can be set up by just adding a where clause

create index blah on table(column) where active = 1

- There is now support for instantly adding columns with default values through the use of sparse values. Cool.
- Index views have been around for a while and are designed to speed up aggregate queries
- Creating a view with the keyword schemabinding will prevent changing the underlying tables to invalidate the view



