---
layout: post
title: S3 backup - Part II - Bucket Policy
authorId: simon_timms
date: 2011-01-04
---

This wasn't going to become a series of posts but it is kind of looking like it is going to be that way. I was a bit concerned about the access to my S3 bucket in which I was backup up my files. By default only I have access to the bin but I do tend to be an idiot and might later change the permissions on this bucket indadvertantly. Policy to the rescue! You can set some pretty complex access policies for S3 buckets but really all I wanted was to add a layer of IP address protection to it. You can set policies by right clicking on the bucket in the AWS Manager and selecting properties. In the thing that shows up at the bottom of your screen select "Edit bucket policy". I set up this policy

  
{  
 "Version": "2008-10-17",  
 "Id": "S3PolicyId1",  
 "Statement": [  
 {  
 "Sid": "IPAllow",  
 "Effect": "Allow",  
 "Principal": {  
 "AWS": "*"   
 },  
 "Action": "s3:*",  
 "Resource": "arn:aws:s3:::bucket/*",  
 "Condition" : {  
 "IpAddress" : {  
 "aws:SourceIp": "255.256.256.256"   
 }  
 }   
 }   
 ]  
}

Yep policies are specified in JSON, it is the new XML to be sure. Replace the obviously fake IP address with your IP address.

This will protect anybody other than me or somebody at my IP from getting at the bucket. I am keeping a watch on the cat just in case he is trying to get into my bucket from my IP.

Next part is going to be about SSL and encryption.



