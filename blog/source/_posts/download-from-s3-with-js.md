---
layout: post
title: Downloading a file from S3 using Node/JavaScript
authorId: simon_timms
date: 2018-09-12
---

There seems to be very scant examples on how to download a file from S3 using node and JavaScript. Here's how I ended up doing it using the `aws-sdk`:

```javascript
const params = {
    Bucket: 'some bucket',
    Key: 'somefilename.txt'
  };

const s3Data = await s3.getObject(params).promise();
console.log('Content length is:' s3Data.ContentLength);
console.log('Content type is:' s3Data.ContentType);
console.log('Writing to file');
const file = fs.createWriteStream('c:/afile.txt');
file.write(s3Data.Body);
```