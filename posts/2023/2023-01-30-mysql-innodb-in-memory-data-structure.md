---
layout: post
title: How does MySQL 5.7 InnoDB In-Memory data structures work?
date: 2023-01-30
---

MySQL maintains an in-memory cache for high throughput and low latency. On dedicated servers, around 80% of memory is utlized by in-memory caching layer to support high-volume read operations.

![InnoDB Architecture](https://dev.mysql.com/doc/refman/5.7/en/images/innodb-architecture-5-7.png)

The components of in-memory data structures are the following

## Buffer Pool

1. The buffer pool is an area in main memory where the INNODB caches the table and index data as it is accessed.
2. In MySQL, data is stored in pages. Pages can contain multiple rows. Buffer pool maintains a linked list of pages.
3. The data that is rarely used is aged out of the cache using a variation of LRU algorithm.
4. Buffer Pool LRU algorithm
   1. A single linked list is divided into two sub-lists i.e young sub-list and old sub-list. Young sub-list is basically 5/8 size of pool while old is 3/8 of size of pool.
   2. A newly accessed page by either user-query or read ahead is inserted in the head of old sub-list.
   3. Young sub-list contains most recently accessed while old sub-list contains most recently accessed pages.
   4. Accessed page gets transferred to young sub-list. Pages not accessed since long reaches the tail of old sub-list and are eventually evicted.

## Change Buffers

1. Change buffer caches changes to the secondary index pages when those indexes are not in buffer pool.
2. These buffered changes may arise from insert, update and delete operations. They are later merged when the pages are loaded into buffer pool through read operations.
3. Overall process of change buffer
   1. Cache changes to secondary indexes not in buffer pool in change buffer.
   2. Periodically write cached index pages to disk.
   3. Changes are merged as soon as secondary indexes are loaded into the buffer pool.
4. Why change buffers stress on secondary indexes change caching?
   1. Reads, writes, deletes and updates on secondary indexes are random as compared to clustered indexes. They are not present adjacently and it is difficult to load so much data into buffer pool.
   2. To save the cost of expensive I/O, changes to secondary indexes are cached in change buffers. These changes are either later flushed to disk or merged when secondary index data is loaded into buffer pool.

## Adaptive Hash Index

1. This enables INNODB to behave like an in-memory database with correct combination of workloads and buffer pool size without sacrificng transactional features.
2. Based on the observed pattern of searches, INNODB builds a hash index from the prefix of an already indexed key. It is built on demand for the pages of the index that are accessed often.
3. INNODB has to maintain a balance between the cost of monitoring the search pattern and benefit of read queries from building an hash index.

## Log Buffer

1. It is the memory area where data to be written to log files on disk is stored temporarily.
2. A large log buffer enables large transactions to run without the need to write to redo log data to disk before the transactions commit.
3. If you have high write throughput, increasing the log buffer size will save disk I/O.
