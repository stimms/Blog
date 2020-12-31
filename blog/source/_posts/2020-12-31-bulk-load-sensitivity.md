---
layout: post
title: Sensitivity classification and bulk load
authorId: simon_timms
date: 2020-12-31 10:00

---

This problem has caught me twice now and both times it cost me hours of painful debugging to track down. I have some data that I bulk load into a database on a regular basis, about 100 executions an hour. It has been working flawlessly but then I got a warning from SQL Azure that some of my columns didn't have a sensitivity classification on them. I believe this feature is designed to annotate columns as containing financial or personal information. 

The [docs](https://docs.microsoft.com/en-us/azure/azure-sql/database/data-discovery-and-classification-overview) don't mention much about what annotating a column as being sensitive actually does except require some special permissions to access the data. However once we applied the sensitivity classification the bulk loading against the database stopped working and it stopped working with only the error "Internal connection error". That error lead me down all sorts of paths that weren't correct. The problem even happened on my local dev box where the account attached to SQL server had permission to do anything and everything. Once the sensitivity classification was removed everything worked perfectly again.  

Unfortunately I was caught by this thing twice hence the blog post so that next time I google this I'll find my own article. 