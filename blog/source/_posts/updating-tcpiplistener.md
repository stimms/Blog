---
layout: post
title: Updating TCPIPListener
authorId: simon_timms
date: 2011-01-07
---

I still have a post on encryption for S3 backups coming but I ran into this little problem today and couldn't find a solution listed on the net so into the blog it goes. I have some code which is using an [obsolete constructor](http://msdn.microsoft.com/en-us/library/1y2a362e.aspx) on System.Net.Sockets.TcpListener. This constructor allows you to have the underlying system figure out the address for you. It became obsolete in .net 1.1 so this is way out of date. In order to use one of the new constructors and still keep the same behavior just use IPAdress.Any.

**Old:**

  
 new System.Net.Sockets.TcpListner(port);//warning: obsolete

**New:**

  
 new System.Net.Sockets.TcpListner(IPAddress.Any, port);



