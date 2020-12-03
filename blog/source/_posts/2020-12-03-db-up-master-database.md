---
layout: post
title: Running DbUp commands against master
authorId: simon_timms
date: 2020-12-03 10:00

---

I ran into a little situation today where I needed to deploy a database change that created a new user on our database. We deploy all our changes using the fantastic tool [DbUp](http://dbup.github.io/) so I figured the solution would be pretty easy, something like this:

```sql
use master;
create login billyzane with password='%XerTE#%^REFGK&*^reg5t';
```

However when I executed this script DbUp reported that it was unable to write to the `SchemaVersions` table. This is a special table in which DbUp keeps track of the change scripts it has applied. Of course it was unable to write to that table because it was back in the non-master database. My databases have different names in different environments (dev, prod,...) so I couldn't just add another `use` at the end to switch back to the original database because I didn't know what it was called. 

Fortunately, I already have the database name in a variable used for substitution against the script in DbUp. The code for this looks like 

```csharp
var dbName = connectionString.Split(';').Where(x => x.StartsWith("Database") || x.StartsWith("Initial Catalog")).Single().Split('=').Last();
var upgrader =
    DeployChanges.To
        .SqlDatabase(connectionString)
        .WithScriptsEmbeddedInAssembly(Assembly.GetExecutingAssembly(), s =>
          ShouldUseScript(s, baseline, sampleData))
        .LogToConsole()
        .WithExecutionTimeout(TimeSpan.FromMinutes(5))
        .WithVariable("DbName", dbName)
        .Build();

var result = upgrader.PerformUpgrade();
```

So using that I was able to change my script to 

```
use master;
create login billyzane with password='%XerTE#%^REFGK&*^reg5t';
use $DbName$;
```

Which ran perfectly. Thanks, DbUp!