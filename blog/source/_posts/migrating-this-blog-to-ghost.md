---
layout: post
title: Shaving the Blog Yak
authorId: simon_timms
date: 2015-01-02
---
I've been on wordpress.com for 2 years now. It does a pretty fair job of hosting my blog and it does abstract away a lot of the garbage you usually have to deal with when it comes to hosting your own blog. However it cost about $100 a year which is a pretty fair amount of money when I have Azure hosting burning a hole in my pocket. There are also a few things that really bug me about wordpress.com.

The first is that I can't control the plugins or other features to the degree that I like. I really would like to change the way that code is displayed in the blog but it is pretty much fixed and I can only do it because I farm out displaying code to github.

Second I'm always super nervous about anything written in PHP. You can shoot yourself in the foot using any programming language but PHP is like gluing the business end of a rail gun to your big toe. One of the most interesting things I've read recently about PHP was [this blog post](http://blog.ircmaxell.com/2014/12/php-install-statistics.html) which suggests that something like 78% of PHP installs are insecure. Hilarious. 

The third and most major thing is that all the content on wordpress.com is locked into wordpress.com. Sure there is an export feature but frankly it sucks and you're really in trouble if you want to move quickly to something else. For instance exporting in a Ghost compatible format requires installing a ghost export plugin. This can't be done because wordpress.com doesn't allow installing your own plugins.

So, having a few days off over Christmas, I decided it was time to make the move. I was also a bit prompted by the Ghost From Source series of posts from Dave Wesst who showed how easy it was to get Ghost running on Azure.

As I expected this whole process was jolly involved. The first thing I wanted was to get an export in a format that Ghost could read. This meant getting the Ghost export plugin working on wordpress.com. This is, of course, impossible. There is no ability to install your own plugins on wordpress.com.

So I kicked up a new Ubuntu Trusty VM on my OSX box using vagrant. I used Trusty because I had an image for it downloaded already. The Vagrantfile looked like

```
# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box = "ubuntu/trusty64"
  config.vm.network "forwarded_port", guest: 80, host: 8000
end
```
Once I was sshed in I just followed the steps for setting up wordpress at https://help.ubuntu.com/community/WordPress.

I dropped the plugins in for WordPress import and for Ghost export. I then exported the WordPress file which is, of course, a giant blob of XML.

The export seems to contain all sorts of data including links to images and comments. Ghost doesn't support either of those things very well. That's fine we'll get to those.

Once I had the wordpress backup installed into my local wordpress I could then use the Ghost export which dumped a giant JSON file. Ah, JSON, already this is looking better than wordpress.

I downloaded the latest Ghost and went about getting it set up locally.

    git clone git@github.com:TryGhost/Ghost.git
    cd Ghost
    npm install
    grunt init
    npm start

This got Ghost up and running locally at http://localhost:2368 . I jumped into the blog at http://localhost:2368/ghost and set up the parameters for my blog. I next went to the labs section of the admin section and imported the JSON dump.

#Issues

I was amazed that it was all more or less working. Things missing were

1. Routes incorrect - old ones had dates in them and the new ones don't
2. Github gists not working
3. Some crazy character encoding issues

They all need to be dealt with.

##Routes

The problem is that the old URLs were like http://blog.simontimms.com/2014/08/19/experiments-with-azure-event-hubs/ while the new ones would be http://blog.simontimms.com/experiments-with-azure-event-hubs/. I found a router folder which pointed me to [frontend.js](https://github.com/TryGhost/Ghost/blob/master/core/server/routes/frontend.js). The contents looked very much like an expressjs router, and in fact that is what it was. Unfortunately the router in express is pretty lightweight and doesn't do parameter extraction. I had to rewrite the url inside the controller itself. Check out the [commit](https://github.com/stimms/Ghost/commit/6859e4a31cfce39c85231324b4e7ba2f469b433a). It isn't pretty but it works.

Later on I was poking around in the settings table of the ghost blog and found that there is a setting called ```permalink``` that dictates how the permalinks are generated. I changed mine from ```/:slug``` to ```/:year/:month/:day/:slug``` and that got things working better without source hackery. This feature is, like most of ghost, a black hole of documentation.

##Gists Not Working

Because there was no good syntax highlighting in WordPress I used embedded gists for code. To do that all you did was put in a github url and it would replace it with the gist.

There are a number of ways to fix this. The first is to simply update the content of the post, the second is to catch the content as it is passed out to the view and change it, the third is to change the templating. It is a tough call but with proper syntax highlighting in ghost I don't really have any need for continuing support for gists. I decided to fix it in the json dump and just reimport everything.

I thought I would paste in the regex I used because it is, frankly, hilarious.

    %s/https:\\\/\\\/gist.github.com\\\/\d\+/<script src='&.js'><\\\/script>/g
    %s/https:\\\/\\\/gist.github.com\\\/stimms\\\/\w\+/<script src='&.js'><\\\/script>/g


##Crazy Character Encoding

Some characters look to have come across as the wrong format. Some UTF jazz, I imagine. I kind of ignored it for now and I'll run some SQL when I have a few minutes to kill.  

#Databases

By default ghost uses an sqlite database on the file system. This is a big problem for hosting on Azure websites. The local file system is not persistent and may disapear at a moment's notice. Ghost is built on top of the Knex data access layer and this data layer works just fine against MySQL as well as Postgresql. There is a free version of MySQL from ClearDB which is of very limited size and very limited number of connections. This is actually fine for me because I don't have a whole lot of people reading my blog (tell your friends to read this blog). 

So I decided to use MySQL and so began a yak shaving to rival the best. The first adventure was to figure out how to set the connection string.  Of course, this isn't documented anywhere, but this is the configuration file I came up with:

```
database: {
            client: 'mysql',
            connection: {
              host: 'us-cdbr-azure-west-a.cloudapp.net',
              user: 'someuser',
              password:'somepassword',
              database:'simononline',
              charset: 'utf8',
              debug: false
          }
        },
```

I logged into the ghost instance I had on azure websites and found that it worked fine. However, when I attempted to import the dump into MySQL I was met with an error. As it turns out the free MySQL has a limit of 4 connections and the import seems to use more than that. I feel like it is probably a bug in Ghost that it opens up so many connections but probably not one that really needs a lot of attention. 

And so began the shaving. 

I jumped back over to my vagrant Ubuntu instance and found it already had MySQL installed as a dependency for Wordpress. I didn't, however, know the password for it. So I had to figure out how to reset the password for MySQL. I followed the instructions at https://dev.mysql.com/doc/refman/5.0/en/resetting-permissions.html

That got the password changed but I couldn't log into it from the host system still. Turns out you have to grant access specifically from that host. 

    SET PASSWORD FOR 'root'@'10.0.2.2' = PASSWORD('fish');

Finally I got into the database and could point my local Ghost install at it. That generated the database and I could import into it without worrying too much about the number of connections opened. 

Then I ran ```mysqldump``` to export the content of the databaes into a file. Finally I sucked this into the azure instance of MySQL. 

#Coments

Ghost has no comments but you can use Disqus to handle that for you. I'm pretty much okay with that so I followed the instructions here:

https://help.disqus.com/customer/portal/articles/466255-exporting-comments-from-wordpress-to-disqus

and got all my comments back up and running. 

#Conclusion
So everything is moved over and I'm exhausted. There are still a couple of thing to fix

1. No SSL on full domain only the xxx.azurewebsites.net one
2. Images still hosted on wordpress domain
3. No backups of mysql yet

I can tell you this jazz cost me way more time than $100 but it is a victory and I really needed a victory. 
