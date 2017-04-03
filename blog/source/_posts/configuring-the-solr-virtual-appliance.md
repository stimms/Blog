---
layout: post
title: Configuring the Solr Virtual Appliance
authorId: simon_timms
date: 2011-08-18
---

I ran into a situation today where I needed a search engine in an application I was writing. The data I was searching was pretty basic and I could have easily created an SQL query to do the lookups I wanted. I was, however, suspicious that if I implemented this correctly there would be a desire for applying the search in other places in the application. I knew a little bit about Lucene so I went and did some research on it reading a number of blogs and the project documentation. It quickly became obvious that keeping the index in sync when I was writing new documents from various nodes in our web cluster would be difficult. Being a good service bus and message queue zealot that seemed like an obvious choice: throw up some message queues and distribute updates. Done.

I then came across [Solr ](http://lucene.apache.org/solr/)which is a web version of Lucene. Having one central search server would certainly help me out. There might be scalability issues in some imaginary future world where users made heavy use of search but in that future world I am rich and don't care about such thing being far too busy plotting to release dinosaurs onto the floor of the New York Stock Exchange.

I was delighted to find that there exists a [virtual appliance](http://susegallery.com/a/Kr7Ayv/blaze-appliance-for-solr) with Solr installed on it already. If you haven't seen a virtual appliance before it is an image of an entire computer which is dedicated to one task and comes preconfigured for it. I am pretty sure this is where a lot of server hosting is going to end up in the next few years.

Once I had the image downloaded and running in Virtual Box I had to configure it to comply with my document schema. The installed which is installed points at the example configuration files. This can be changed in /usr/share/tomcat6/conf/Catalina/localhost/solr.xml, I pointed it at /usr/local/etc/solr. The example configuration files were copied into that directory for hackification.

<span style="background-color:#f3f3f3;padding-left:10px;">  
  
 cp -r /usr/share/apache-solr-1.4.1/example/solr/* /usr/local/etc/solr/  
  
</span>

Once you've got these files in place you can crack open the schema.xml and remove all the fields in there which are extraneous to your needs. You'll also want to remove them from the copyField section. This section builds up a super field which contains a bunch of other fields to make searching multiple fields easier. I prefer using this [DisMax ](http://wiki.apache.org/solr/DisMaxQParserPlugin)query to list the fields I want to search explicitly.



