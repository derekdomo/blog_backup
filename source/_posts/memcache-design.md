
---
title: Key Value Store Design
date: 2020-02-18
categories: [Interview]
tags: [System Design]
---

Key Value Data Store猛的一看，其实是一个很简单系统，没有什么特别复杂的逻辑，基本上是两个功能：
* Lookup
* Insert
但是里面还是有很多很tricky的地方的，也有很多很常见的系统设计的optimization相关的技术在里面使用。

## Single Server Implementation
Single server特别简单，就是一个hashmap的功能，但是面试官可能会问你几个常见的hashmap的考点：
* How to avoid memory fragmentation
* How to handle concurrent read and write
* How to design collision array and when should we expand it
* What to do if we are reaching limit of the memory

### Memory Framentation
* 这方面是什么意思呢？简单来说，如果我们insert的value的大小不一，这样子当一些小的value消失之后，他们所占用的小空间是无法被大的value所利用的，就会导致memory fragmentation。
* 这样子需要我们合理的分配memory，可参考memcache是如何optimize这方面的，他们把RAM分为page，然后在上面虚拟出了slab的概念，然后slab呢，又分大slab和小slab，然后根据不同的slab的使用情况去选取pages。

### Concurrent Read and Write
* 这个就比较简单直接了，就是一个锁的问题。一般来说会用mutex，但是也可以不用mutex，单纯的就是single thread。
* Redis就是single thread，而memcache就允许使用多个thread，但是也有上限。
* 如果使用锁的话，我们可以在key上加mutex

### Expanding the table
* 这里有一个概念叫做load factor，根据这个我们可以来判断什么时候我们该做expand操作
* 典型的expand操作是一个一个的移过去，然后读是读两个，写是写新的那个

### Eviction Policy
* 这方面有很多算法，LRU，LFU，等等


## Multiple Server Implementation
这就更多的属于distributed system的问题了，主要的问题有以下几种：
* how to load balance
* how to avoid potential issues while elastically adding new machines

