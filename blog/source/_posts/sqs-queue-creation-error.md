---
layout: post
title: Error creating an SQS queue in serverless
authorId: simon_timms
date: 2018-09-05
---

I ran into an error today creating an SQS queue in serverless which looked a lot like: 

```
CloudFormation - CREATE_FAILED - AWS::SQS::Queue - SendEmailQueue
...
An error occurred: SendEmailQueue - API: sqs:CreateQueue Access to the resource https://sqs.us-east-1.amazonaws.com/ is denied..
```

You would think this error is related to something in IAM, and indeed it may well be. However in my case it wasn't it was actually that I was giving CloudFormation a mal-formed name for the queue. I would like to think that the developers of SQS would use HTTP code 400 (bad request) to report this error but instead they use 403. So keep in mind that even though it looks like a permissions problem it might be a syntax error.