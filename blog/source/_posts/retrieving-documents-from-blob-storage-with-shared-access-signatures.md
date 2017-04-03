---
layout: post
title: Retrieving Documents from Blob Storage with Shared Access Signatures
authorId: simon_timms
date: 2013-06-25
---

Azure blob storage provides a place where you can put large binary objects and then retrieve them relatively quickly. You can think of it it as a key-value store, if you like. I don't know how many people use it for storing serialized objects from their applications but I bet hardly any. SQL storage is a far more common approach and even table storage with its built in serialization seems better suited for most storage of serializable data. For the most part people use it as a file system, at least I do. When there is a need to upload a document in my program the server side application takes the multi-part form data and shoves it into blob storage. When it needs to be retrieved, a far more common activity, then I like to let blob storage itself to the heavy lifting.

Because blob storage is accessed over HTTP you can actually give out the document name from blob storage and let access blob storage directly. This saves you on bandwidth and server load since you don't have to transfer it to your server first and it is almost certainly faster for your clients.

<div class="wp-caption aligncenter" id="attachment_2888" style="width: 760px">[![From blob storage, to cloud server to end user](http://stimms.files.wordpress.com/2013/06/traditional-blob.png?w=750)](http://stimms.files.wordpress.com/2013/06/traditional-blob.png)From blob storage, to cloud server to end user

</div>You can set up access control on blob storage to allow for anonymous read of the blob contents. However that isn't necessarily what you want because now everybody can read the contents of that blob. What we need is a way to instruct blob storage to let people read a file but only specific groups of people. The common solution to this problem is to give people a one time key to access the blob storage. In azure world this is called a Shared Access Signature Token. I wouldn't normally blog about this because there are a million other sources but I found that the terminology has changed since the 2.0 release of the Azure tools and now all the other tutorials are out of date. It took me a while to figure it all out so I thought I would save some time.

[![blob direct](http://stimms.files.wordpress.com/2013/06/blob-direct.png?w=750)](http://stimms.files.wordpress.com/2013/06/blob-direct.png)The first set is to generate a token.

<script src='https://gist.github.com/stimms/5859897.js'></script>

Here I set up the blob security to remove public access then I generate aSharedAccessBlobPolicy to have an expiration time 1 minute in the past and 2 minutes in the future. The time in the past is to account for minor deviations in the clocks on the server and on blob storage. This policy is then assigned to the container.

<script src='https://gist.github.com/stimms/5859967.js'></script>

The URL returned looks something likehttp://127.0.0.1:10000/devstoreaccount1/documents/cdb8335d-f001-46ca-84da-de12ac57157b?sv=2012-02-12&sr=c&si=temporaryaccesspolicy&sig=BC4YjrFtVfpkZry9VHCB9qDMKqGS4%2B46rcNt30kjH4o%3D

Fun! Let's break it down

This part is the blob storage url(this one is against local dev storage and my blob id is a guid to prevent conflicts)

http://127.0.0.1:10000/devstoreaccount1/documents/cdb8335d-f001-46ca-84da-de12ac57157b

The second part here is the SAS Token which is valid for the next 2 minutes. Hurry up!

?sv=2012-02-12&sr=c&si=temporaryaccesspolicy&sig=BC4YjrFtVfpkZry9VHCB9qDMKqGS4%2B46rcNt30kjH4o%3D

If you attempt to retrive the document after the time window then you'll get this error

<script src='https://gist.github.com/stimms/5860013.js'></script>

While this method of preventing people from getting the protected documents is effective it isn't flawless. It relies on providing a narrow window during which an attack can function. If you have several classes of documents, say one for each customer of your site, then it would be advisable to isolate each customer in their own blob container.



