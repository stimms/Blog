---
layout: post
title: Configuration Settings in an Azure Worker Role
authorId: simon_timms
date: 2013-08-28
---

I have found that developing an Azure worker role is somewhat poorly documented. Perhaps I am just not good at googling for what I need but that has not been my experience in the past. Anyway I have a few worker roles in a project I've been struggling with how to get connection strings into them. I need two strings: a database connection and an azure storage connection string. Typically I would set this up by having an app.config file but that doesn't scale particularly well out to the cloud. You have to redeploy to change of the settings. Instead I thought I would make use of the settings mechanism provided by Azure.

The first step is to set up the settings in your Azure project. This is done by opening up the properties for the role and going to the settings tab

<div class="wp-caption aligncenter" id="attachment_2975" style="width: 760px">[![Good try, Harriet the Spy, you can't see anything secret in this screenshot.](http://stimms.files.wordpress.com/2013/08/screen-shot-2013-08-28-at-11-05-15-am.jpg?w=750)](http://stimms.files.wordpress.com/2013/08/screen-shot-2013-08-28-at-11-05-15-am.jpg)Good try, Harriet the Spy, you can't see anything secret in this screenshot.

</div>I added two settings: StorageConnectionString and DefaultConnection. While writing this I decided that I hate both those names, drat. You can pick the environment in the service configuration drop down. Cloud is used in the cloud and local is used during a locally emulated cloud.

In my code I created a static helper class to access these settings

<script src='https://gist.github.com/stimms/6368512.js'></script>

You can see that I'm checking two sources for the information. If the role environment provider works then I use that connection string otherwise I use the fallback to the configuration file. For some reason the role environment bails with a full on exception if you try to get configuration information out of it without it running in the cloud or emulator. That seems like overkill, especially because the exception thrown is super general and provides no helpful information.

The config file settings are used when I'm running tests on the service locally outside of the emulator. Frequently the emulator is overkill for the simple debugging I'm doing, so I have a unit test which can be enabled that just launches the service. It is quick and close enough to production for most purposes.

In azure proper you can configure overrides for the cloud settings in the configure tab of your cloud service.

<div class="wp-caption aligncenter" id="attachment_2976" style="width: 760px">[![In Azure you can update the configuration settings.](http://stimms.files.wordpress.com/2013/08/screen-shot-2013-08-28-at-11-19-38-am.jpg?w=750)](http://stimms.files.wordpress.com/2013/08/screen-shot-2013-08-28-at-11-19-38-am.jpg)In Azure you can update the configuration settings.

</div>All this seems to work pretty well.



