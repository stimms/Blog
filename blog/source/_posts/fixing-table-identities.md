---
layout: post
title: Fixing Table Identities
authorId: simon_timms
date: 2010-06-28
---

I make heavy use of Red Gate's excellent SQL Compare tools. I know I'm a bit of a shrill for them but they are time savers when dealing with multiple environments(development, testing, production) which is a pretty common occurrence in any sort of agile development. One flaw in them is that they often mess up the sequences in the destination database. Say you have a table Students with 15 records in it in development and 30 in production then performing a copy often brings along the sequence even if you don't select syncing that table. This results in duplicate key errors whenever a new record is inserted.

For weeks I have been saying â€œI should write a script to check and fix thatâ€. Well I finally did it

  
SET ANSI_NULLS ON  
GO  
SET QUOTED_IDENTIFIER ON  
GO  
  
create PROCEDURE dbo.FixTableIdentities  
  
AS  
BEGIN  
 SET NOCOUNT ON;  
 declare @currentKeyValue int  
 declare @currentIdentityValue int  
 declare @toRun nvarchar(500)  
 declare @tableToCheck nvarchar(500)  
 declare @idColumnCount int  
 declare db_cursor cursor for select name from sysobjects where type='U'  
  
 open db_cursor  
 fetch next from db_cursor into @tableToCheck  
  
 while @@FETCH_STATUS = 0  
 BEGIN  
 select @idColumnCount = count(*) from syscolumns where id=object_id(@tableToCheck) and name='id'  
 if(@idColumnCount = 1)  
 BEGIN  
 select @currentKeyValue = ident_current(@tableToCheck)   
 set @toRun = N'select @currentIdentityValue = max(id) from ' + @tableToCheck;  
 EXEC sp_executesql @toRun, N'@currentIdentityValue int OUTPUT', @currentIdentityValue OUTPUT;  
 if(@currentIdentityValue @currentKeyValue)  
 BEGIN  
 DBCC CHECKIDENT (@tableToCheck,reseed, @currentIdentityValue)   
 END  
 END  
 FETCH NEXT FROM db_cursor into @tableToCheck  
 END  
 CLOSE db_cursor  
 deallocate db_cursor  
END  
GO

When run this procedure will go through all your tables and ensure that the id column is in sync with the sequence. At the moment it just looks at the column called id and manipulates that.



