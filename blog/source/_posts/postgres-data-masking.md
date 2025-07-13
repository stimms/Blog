---
layout: post
title: Postgres Data Masking and Anonymization
authorId: simon_timms
date: 2025-07-12
---

# Postgres Data Masking and Anonymization

Predicting the performance of a web application is always a little bit difficult. If it's a question of how the site will perform under load then you can use things like [Artillery](https://www.aspnetmonsters.com/2019/08/monsters-weekly%5Cep126/) to throw requests at it. But sometimes problems arise from increasing amounts of data. This was the case for a site I helped develop a little while back.

It had been in production for a couple of years and was starting to have problems with some of the queries. I profiled some of them and found some query optimizations which cleared it up. But it was annoying that this hadn't been caught earlier and that it was difficult to replicate in lower environments because they simply didn't have enough data.

We could have generated data but for any sort of complex data model it's difficult to create realistic data. Fortunately I knew of a place we could get really well structured data which would have the same performance profile as production: production. But this data contains sensitive information like phone numbers, addresses names and salaries. I didn't want to just copy that over to the lower environments so I needed a way to clean up the data. 

Initially I started with just throwing [FakerJS](https://fakerjs.dev/) at the data but performance of updating every row with values was not great. After some research I found the Postgres extension [PostgreSQL Anonymizer](https://postgresql-anonymizer.readthedocs.io/en/stable/) which looked like it would fit the bill.

# PostgreSQL Anonymizer

PostgreSQL Anonymizer is a Postgres extension which allows you to mask data in a variety of ways. It can be used to anonymize data or to mask it. It has a number of built-in functions for common tasks like replacing names with random names, addresses with random addresses and so on.

Getting it installed on Azure Postgres Flexible Server was a little tricky and I'll probably post on that in a separate article. But once it was installed it was easy to use. 

There are a couple of modes it can run in Dynamic and Static. In the dynamic mode it will leave the data in the table put but will apply rules based on the current user role to mask the data when queried. This is a pretty handy thing and you could use it for something like masking SSNs for any user other than the admin. Static mode will actually update the data in the table to a new value, per your rules. This is what I opted for as by default the masking wasn't deterministic. 

# Using PostgreSQL Anonymizer

The script I put together to run after I had restored a backup into the test environment looked like this:

```sql
-- Start up the extension
CREATE EXTENSION anon;

-- Rules
SECURITY LABEL FOR anon ON COLUMN household.contact_email
  IS 'MASKED WITH FUNCTION anon.fake_email()';
SECURITY LABEL FOR anon ON COLUMN household.contact_phone
  IS 'MASKED WITH FUNCTION anon.random_int_between(10000000,90000000)';

SECURITY LABEL FOR anon ON COLUMN address.street1
  IS 'MASKED WITH FUNCTION anon.fake_address()';
SECURITY LABEL FOR anon ON COLUMN address.street2
  IS 'MASKED WITH FUNCTION anon.fake_address()';

...

-- Anonomize database statically
SELECT anon.anonymize_database();
```

You can see the sections there first enable the extension, then create a series of rules which will replace the data in the columns with fake data. Finally  `SELECT anon.anonymize_database();` will actually kick off the changes. A few seconds of crunching later and the data is all faked up and we can hand over the database to QA or developers without having to worry about sensitive data leaking. 