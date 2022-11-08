---
layout: post
title: How to generate Kafka cluster ID?
date: 2022-11-08
---

Generate Kafka cluster ID for Kafka Raft(KRaft) metadata mode.

Kafka cluster ID is expected to be 16 bytes of a base64-encoded UUID.

## 1. Method 1

You can generate cluster ID using Kafka official utilities.

```sh
$ cd /opt/kafka/kafka/
$ ./bin/kafka-storage.sh random-uuid
M2UyYjBlNDdiNjI4NDYyYm
```

## Method 2

Alternatively, use `proc` filesystem, which comes in handy when you want to dynamically generate multiple clusters without locally extracting Kafka archive to generate UUIDs.

```sh
$ cat /proc/sys/kernel/random/uuid | tr -d '-' | base64 | cut -b 1-22
```

## Method 3

There is also `uuidgen` program, which gives you more control over the process as you can generate random-based or time-based UUIDs.

```sh
$ man uuidgen

...
OPTIONS
       -r, --random
              Generate a random-based UUID.  This method creates a UUID consisting mostly of random bits.  It requires that the operating system have a high quality random number generator, such as /dev/random.

       -t, --time
              Generate a time-based UUID.  This method creates a UUID based on the system clock plus the system's ethernet hardware address, if present.
...
```

```sh
$ uuidgen --time | tr -d '-' | base64 | cut -b 1-22 
```

## Format Kafka storage

```sh
$ /opt/kafka/kafka/bin/kafka-storage.sh format -t M2UyYjBlNDdiNjI4NDYyYm -c /opt/kafka/kafka/config/kraft/server.properties
Formatting /opt/kafka/kafka/kafka_data

$ cat /opt/kafka/kafka/kafka_data/meta.properties

...
node.id=1
version=1
cluster.id=N2Q0ZTNlMWFhMTgyNDhiZj
...
```
