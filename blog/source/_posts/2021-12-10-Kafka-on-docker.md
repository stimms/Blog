---
layout: post
title: Kafka and .NET - Part 2 - Launching Kafka on Docker
authorId: simon_timms
date: 2021-12-14 10:00
---


The fastest way to get started with your own Kafka cluster is to launch Kafka on Docker. It saves you from configuring anything locally but still gives you the ability to scale to multiple nodes on your machine. 

<!--more -->

<div style="background: aliceblue; padding-bottom: 20px"><h2>C# Advent</h2>This post is one among many which is part of 2021's [C# Advent](https://csadvent.christmas/). There are a ton of really great posts this year and some new bloggers to discover. I'd strongly encourage you to check it out.</div>

Because there are a couple of pieces to Kafka you'll probably want to launch not one but two containers. The first will hold Zookeeper and the second Kafka proper. Here is a simple `docker-compose.yml` file for it 

```docker
version: '2'
services:
  zookeeper:
    image: confluentinc/cp-zookeeper:latest
    environment:
      ZOOKEEPER_CLIENT_PORT: 2181
      ZOOKEEPER_TICK_TIME: 2000
    ports:
      - 22181:2181
  
  kafka:
    image: confluentinc/cp-kafka:latest
    depends_on:
      - zookeeper
    ports:
      - 29092:29092
    environment:
      KAFKA_BROKER_ID: 1
      KAFKA_ZOOKEEPER_CONNECT: zookeeper:2181
      KAFKA_ADVERTISED_LISTENERS: PLAINTEXT://kafka:9092,PLAINTEXT_HOST://localhost:29092
      KAFKA_LISTENER_SECURITY_PROTOCOL_MAP: PLAINTEXT:PLAINTEXT,PLAINTEXT_HOST:PLAINTEXT
      KAFKA_INTER_BROKER_LISTENER_NAME: PLAINTEXT
      KAFKA_OFFSETS_TOPIC_REPLICATION_FACTOR: 1
```

You can just drop this file into a text file and run 

```bash
docker-compose up -d
```

To launch it. You should now have a single-node cluster on port 29092 that you can access from the .NET Client. As this is a single node configuration so you're not going to be able to test cluster operation like setting the number of replicas or having partitions on multiple machines. If you want to do that you can adjust the compose file to have more than one kafka instance. 


```
version: '2'
services:
  zookeeper:
    image: confluentinc/cp-zookeeper:latest
    environment:
      ZOOKEEPER_CLIENT_PORT: 2181
      ZOOKEEPER_TICK_TIME: 2000
    ports:
      - 22181:2181
  
  kafka1:
    image: confluentinc/cp-kafka:latest
    depends_on:
      - zookeeper
    ports:
      - 29092:29092
    environment:
      KAFKA_BROKER_ID: 1
      KAFKA_ZOOKEEPER_CONNECT: zookeeper:2181
      KAFKA_ADVERTISED_LISTENERS: PLAINTEXT://kafka1:9092,PLAINTEXT_HOST://localhost:29092
      KAFKA_LISTENER_SECURITY_PROTOCOL_MAP: PLAINTEXT:PLAINTEXT,PLAINTEXT_HOST:PLAINTEXT
      KAFKA_INTER_BROKER_LISTENER_NAME: PLAINTEXT
      KAFKA_OFFSETS_TOPIC_REPLICATION_FACTOR: 1
  kafka2:
    image: confluentinc/cp-kafka:latest
    depends_on:
      - zookeeper
    ports:
      - 29093:29093
    environment:
      KAFKA_BROKER_ID: 2
      KAFKA_ZOOKEEPER_CONNECT: zookeeper:2181
      KAFKA_ADVERTISED_LISTENERS: PLAINTEXT://kafka2:9093,PLAINTEXT_HOST://localhost:29093
      KAFKA_LISTENER_SECURITY_PROTOCOL_MAP: PLAINTEXT:PLAINTEXT,PLAINTEXT_HOST:PLAINTEXT
      KAFKA_INTER_BROKER_LISTENER_NAME: PLAINTEXT
      KAFKA_OFFSETS_TOPIC_REPLICATION_FACTOR: 1
  kafdrop:
      image: obsidiandynamics/kafdrop:latest
      container_name: kafdrop
      ports:
        - 9000:9000
      environment:
        - KAFKA_BROKERCONNECT=kafka1:9092,kafka2:9093
      depends_on: 
        - kafka1
      
```

I also found out about this nifty little Kafka monitoring UI called Kafdrop from Sascha Muller's blog post here: https://blog.exxeta.com/en/2021/05/20/kafka-cluster-setup-with-docker-and-docker-compose/ This tool gives you some basic cluster statistics and also lets you manually create topics. My topic is about goats. 

![Kafdrop dashboard](/images/kafka/2021-12-05-16-41-11.png)

In the next post we'll finally get to .NET portion of this. It should be fun so stay tuned!

