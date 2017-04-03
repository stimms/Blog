---
layout: post
title: Can't connect to windows docker daemon
authorId: simon_timms
date: 2016-07-20
---
I updated my Windows machine to the latest version on the fast ring to get some access to awesome Windows container goodness. I followed the instructions at [Microsoft's MSDN](https://msdn.microsoft.com/en-us/virtualization/windowscontainers/quick_start/quick_start_windows_10?f=255&MSPPError=-2147217396) but I got stuck trying to connect to the docker daemon to import the image.

```
C:\Users\Simon> docker load -i nanoserver.tar.gz
An error occurred trying to connect: Post http://localhost:2375/v1.21/images/load: dial tcp 127.0.0.1:2375: ConnectEx tcp: No connection could be made because the target machine actively refused it.
```

Turns out the solution is to put a file in `c:\programdata\docker\config\daemon.json` and inside that file put 
```
{
    "hosts": ["tcp://0.0.0.0:2375"]
}
```

This will listen on any interface on port 2375. You might do better to put in 
```
{
    "hosts": ["tcp://127.0.0.1:2375"]
}

```
which will at least limit connections to your local machine. Now everything else in the tutorial works as it should. 
