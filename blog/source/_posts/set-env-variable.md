---
title:  Setting a persistent environment variable 
authorId: simon_timms
date: 2021-05-07
mode: public
---



If you want to set a variable but you want it to live forever then you can use

```powershell
[System.Environment]::SetEnvironmentVariable("JAVA_HOME", "c:\program files\openjdk\jdk-13.0.2", "Machine")
```

That last argument can take on the values {`Process`, `User`, `Machine`}