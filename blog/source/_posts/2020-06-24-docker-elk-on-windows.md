---
layout: post
title: Running Dockerized ELK Stack on Windows
authorId: simon_timms
date: 2020-06-24 10:00

---

The [ELK stack](https://www.elastic.co/what-is/elk-stack) is a combination of 3 tools which make log ingestion and search a snap. I needed to do some debugging on a Kibana dashboard for a client so I tried to stand up a quick docker container on Windows to try out the queries. To do this I used 

```
sudo docker pull sebp/elk
docker run -p 5601:5601 -p 9200:9200 -p 5044:5044 -it --name elk sebp/elk
```

During startup Elasticsearch failed to start with 

```
ERROR: [1] bootstrap checks failed
[1]: max virtual memory areas vm.max_map_count [65530] is too low, increase to at least [262144]
ERROR: Elasticsearch did not exit normally - check the logs at /var/log/elasticsearch/elasticsearch.log
```

Fortunately this is really quick to fix with my WSL based docker set up. I just started WSL directly and issued 

```
> sudo sysctl vm.max_map_count
vm.max_map_count = 65530
```

This found that indeed I had too few of a max_map_count. Fixing it required issuing

```
> sudo sysctl -w vm.max_map_count=262144
```

Elasticsearch and, in fact, all of ELK started up nicely after that. 