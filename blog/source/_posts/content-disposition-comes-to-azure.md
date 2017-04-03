---
layout: post
title: Content-Disposition comes to azure
authorId: simon_timms
date: 2013-12-02
---

The Azure team accepts requests for new features on their user voice voice page. I have spent an awful lot of votes on this request "[Allow Content-Disposition http header on blobs](http://feedback.windowsazure.com/forums/217298-storage/suggestions/3866819-allow-content-disposition-http-header-on-blobs)". Well now it has been released!

Why am I so excited about this? Well when I put files up into storage in order to avoid name conflicts I typically use a random file name such as a GUID and then store that GUID somewhere so I can easily look up the file and access it. The problem arises when I try to let people directly download file from blob storage, they get a file which is named as a random string of characters. That isn't very user friendly. Directly accessing blob storage lets me offload the work from my web servers and onto any number of storage servers. So I don't want to abandon that either. Content disposition lets me hijack the name of the file which is downloaded.

To make use of the new header is actually very simple. I updated the save to blob storage method in the project to take an optional file name which I push into the content disposition

<script src='https://gist.github.com/stimms/7728795.js'></script>

Now when I link people directly to the blob they get a sensibly named file.To do this you'll need the latest Azure storage dll(3.0).One note of caution is that as of writing the storage emulator hasn't been updated and will throw some odd errors if you attempt to use the new storage dll against it. Apparently it will all be fixed in the next release of the emulator.

Setting the content disposition header on the blob ensures that everybody who downloads it gets the renamed file. It is also possible to set the header using the shared access signature (SAS) so that you can modify the name of the document for each download. Although, I'll be honest, I could not find a way of doing this from the managed storage library. I can only find examples using the REST API.





