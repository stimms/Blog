---
layout: post
title: Elasticsearch
authorId: simon_timms
date: 2013-06-24
---

I have [previously written](http://blog.simontimms.com/2013/02/27/search-everywhere-is-the-future/ "Search Everywhere is theFuture") about how I think that search is the killer interface of the future. We're producing more and more data every day and search is the best way to get at it. In the past I've implemented search both in a naÃ¯ve fashion and using lucene, a search engine. In thenaÃ¯ve approach I've just done wildcard searches inside a SQL database using a like clause

select * from table_to_search where search_column like '%' + @searchTerm + '%'

This is a very inefficient way to search but when we benchmarked it the performance was well within our requirements even on data sets 5x the size of our current DB. A problem with this sort of search is that it requires that people be very precise with their search. So if the column being searched contains "I like to shop at the duty free shop" and they search for "duty-free" then they're not going to get the results for which they're looking. It is also very difficult to scale this over a large number of search columns you have to keep updating the query.

Lucene is an Apache project to provide a real search engine. It has support for everything you would expect in a search engine: stemming, synonyms, ranking,"¦ It is, however, a bit of a pain tointegratewith your existing application. That's why I like [Elasticsearch](http://www.elasticsearch.org/) so much. It is an HTTP front end for Lucene.

I like having it as a search appliance on projects because it is just somewhere I can dump documents to be indexed for future search even if I don't plan on searching the data right away.

Setting up a basic Elasticsearch couldn't be simpler. You just need to download the search engine and start it with the binary in the bin directory.

bin/elasticsearch

This will start an HTTP server on port 9200(this can be configured, of course). Add documents to the collection using HTTP PUT like so

curl -PUT 'http://localhost:9200/documents/tag/1' -d '{ "user" : "simon", "post_date" : "2013-06-24T11:46:21", "number" : "PT-0093-01A", "description" : "Pressure transmitter" }'

The URL contains the index (documents) and the type (tag) as well as the id(1). To that we can put an arbitrary json document. If the index documents doesn't already exist it will be created automatically with a set of defaults. These are fine defaults but lack any sort of stupport for stemming orlemming. You can set these up using the index creation API. I found a good example which demonstrates some more advanced index options:

<script src='https://gist.github.com/stimms/5852701.js'></script>

In this example a snowball stemming filter is used to find word stems. This means that searching for walking in an index which contains only documents with walk will actually result in results. However stemming does not look words up in a dictionary, unlike lemming, so it won't find good if you search for better. Stemming simply modified words according to an algorithm.

There are a number of ways to retrieve this document. The one we're interested in is, of course, search. A very simple search looks like

curl -XGET 'http://localhost:9200/documents/_search?pretty=true' -d '{ "query": { "fuzzy" : {"_all": "pressure"} } }

This will perform a fuzzy search of all the fields in our document index. You can specify an individual field in the fuzzy object by specifying a field instead of _all. You'll also notice that I append pretty=true to the query, this just produces more readable json in the result.

Because everything is HTTP driven you might be tempted to have clients query directly against the ElasticSearch end point. However that isn't recommend, instead it is suggested that the queries be run by a server against an ElasticSearch instance behind a firewall.

Adding search to an existing application is as easy as setting up an ElasticSearch instance. ElasticSearch can scale up over mulitple nodes if needed or you can use multiple indicies for different applications. So whether you're big or small ElasticSearch can be a solution for searching.



