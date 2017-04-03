---
layout: post
title: Entity Framework Code First
authorId: simon_timms
date: 2013-05-09
---

This is another dump of my notes from a talk at Prairie Dev Con. This is David Paquette's Entity Framework Code First talk which is, oddly, subtitled Magic Unicorn. Very odd.

<div class="wp-caption aligncenter" id="attachment_2692" style="width: 760px">[![David is also a beautiful unicorn ](http://stimms.files.wordpress.com/2013/05/unicorn.jpg?w=750)](http://stimms.files.wordpress.com/2013/05/unicorn.jpg)David is also a beautiful unicorn

</div>- <span style="line-height:13px;">EF shipped with VS 2008 and was junk</span>
- ORM is tough, please don't write your own
- There are 3 flavours of EF model first, database first and code first.
- Model first using a visual designer to build up a model of the data. Kind of like LINQ
- Database first will generate you a model from an existing database
- Code first you create POCOs first and use conventions to figure out things like the key and foreign key references
- Conventions - virtual IList<Something> or IColleciton<Something> creates a foreign key to the collection of Somethings.
- virtual Something creates a one to one mapping from the current object to a Something
- any property called Id becomes a primary key
- EF will create a database using the name of the DbContext if there is no configured database
- Good idea to initialize collections in the constructor. Using a HashSet is more efficient than using a List because hash set optimizes for having no duplicates.
- A DbContext provides a mapping from the objects to the database. Each entity is just set up as an IDbSet in the DbContext.
- The LINQ queries return IQueryables which are a built up collection of lambads which are applied in a delayed or lazy fashion
- It is important to convert the IQuerable to an array or list before passing the data to the view as the DbContext may be out of scope before the view is rendered
- You should dispose your DbContext as that returns the connection to the pool. This is best done using a handy, dandy injection framework. In this case Ninject.
- To return a single object you might call _dbContext.Somethings.Find(id) you can also use Single or First which have slightly different failure criteria.
- To add an entity use an Add on the context and then call SaveChanges to synchronize the DbContext with the database
- Tidbit: There are no performance implications to using varchar(max) except that varchar(max) cannot be indexed so it can't be easily searched.
- When editing an entity you need to mark the state as modified _dbContext.Somethings(entity).State = EntityState.Modified.
- You can specify a database initializer where you can have it delete and recreate a database or add default data
- You can profile your applications using MiniProfiler.MVC which is a lightweight profiler from Stackoverflow. To hook in EF include MiniProfiler.EF
- You can to eager loading by specifying .Include("Something.Comments") this would load the comments collection for Something
- Projections can be used to trim down what is returned form the database
- Paging is implemented using Skip and Take

Thoughts:

EF is a lot of magic but it seems like pretty well thought out and easily overridable magic.



