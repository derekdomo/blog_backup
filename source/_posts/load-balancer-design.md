
---
title: Load Balancer Design
date: 2020-02-19
categories: [Interview]
tags: [System Design]
---

Load Balancer是一个很基础且实际的概念，主要需要解决三个问题
* distribute requests/network load efficiently across servers
* High availabilities & reliabilities
* Flexible to add and remove servers

# Distribute Requests Efficiently
## Level 4 vs Level 7
一般来说有两种的load balancer，level 4 Transport Level和Level 7 Application Level。各有优劣，level4的比较贵，scale起来成本比较高。而Level 7就会比较便宜一些，而且可以实现更多的功能。

## Sticky Routing/Session Persistence
有些比较特殊的应用场景，会需要某些request去特定的server，这就需要load balancer维系一个cache来maintain这个状态。
至于如何来识别这些request呢，就需要一个identifier，可以是session id，也可以是application level的自己生成的ID。

## Load Balancer Algorithm
这些就是一些比较常见的算法了，round robin啊，least conn啊，consistent hashing。但是要注意consistent hashing他适合的应用场景，和短板。

# High Availabilities and Reliabilities
## Heatbeat
load balancer是一个很critical的component，如果挂了，那就真的挂了。所以需要一个heart beat来做health check来保证他是work的。

## Failover/Redundant LB Server
为了保证High Availabilities，就需要在发现LB挂掉的时候，即使的switch over到另一个LB上，来保证availability。


# Flexible to add and remove servers
First thing first, 如果我们要add和remove server，那就意味着我们要如何存储那些server
* HAProxy是通过configuration file来register backend的。但是我们也要保证我们可以auto scale
