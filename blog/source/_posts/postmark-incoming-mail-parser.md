---
layout: post
title: Postmark Incoming Mail Parser
authorId: simon_timms
date: 2014-02-06
---

If you're deploying a website or tool to Azure and need to send e-mail you may already have discovered that it doesn't work. To send e-mail there are a ton of third party services into which you can hook. I've been making use of [Postmark](https://postmarkapp.com) for my sites. Honestly I can't remember why I picked them. They have a nice API and offer 10 000 free credits. Subsequent credits are pretty reasonably priced too.

One of the cool features they have is the ability to accept incoming e-mail. This opens up a whole new world of interactivity with your application. We have a really smart use for it Secret Project Number 1, about which I can't tell you. Sorry ![:(](http://localhost:8000/wordpress/wp-includes/images/smilies/icon_sad.gif)

When I checked there was no library for .net to parse the incoming mail. Ooop, well there's an opportunity. So I wrote one and shoved it into nuget.

[https://github.com/stimms/PostmarkIncomingMailParser](https://github.com/stimms/PostmarkIncomingMailParser)


# Using it

First you should set up a url in the postmark settings under the incoming hook. This should be the final URL of your published end point. Obviously there cannot be a password around the API as there is nowhere to set it. That might be something Postmark would like to change in the future.

The simplest way to set up a page to be the endpoint is to set up a ASP.net MVC WebAPI page. It can contain just a single method

```
public async void Post()
{
    var parser = new PostmarkIncomingMailParser.Parser();
    var mailMessage = parser.Parse(await Request.Content.ReadAsStringAsync());
    //do something here with your mail message
}
```

The mail message which is returned is an extension of the standard System.Net.MailMessage. It adds a couple of extra fields which are postmark specific

-MessageId "“ the ID of the message from postmark  
 -Date "“ the date of the message (see the issues section)

There is a sample project in git which makes use of WebAPI. And that's all I have to say about that.



