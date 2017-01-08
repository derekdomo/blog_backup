
---
title: Fallacies about Microservices
date: 2016-12-18
categories: [每日一读]
tags: [每日一读]
---

Microservices这个词前段时间算是比较火，尤其是很多小公司，microservices几乎充斥了整个backend services。今天看了一篇文章是对这个服务的优缺点的分析，正好学习一下。

## Background knowledge
一个普通的application一般由这四个部分组成
* presentation components
* business logic component
* database access logic

### Monolithic Services
与这个名词对立的便是monolithic service。在这种architecture中，这三个模块可以是layered，也可以是hexagonal的。但是一般这样子的application是会打成一个Java WAR包，或者像ROR网站一样，Rails和NodeJS在一个code base里面。所以从部署角度上看的话，所有的server上都部署了一样的代码，一样的application，很像数据的fully replication。

### Microservices
然而在microservice中，所有的都像图中的节点一样连在了一起，每个component都是一个独立的service被部署在一台机器上，彼此之间通过REST/RPC来通信。

## Fallacies of Microservices
Microservices被推崇一般是由以下几个原因：
* Cleaner Code
* Easy to write things that only have one purpose
* Faster
* Easy for engineers to not work on whole code base
* Simple to handle autoscalling
接下来我们来一个一个分析这几个优点，看看是不是属实。

### Cleaner Code
每个microservice只需要定义好自己提供给别人的接口，就可以闷头自己开发，不需要受别人的影响，这样子感觉的确是降低程序员为了hustle而写出lazy或者poorly thought out代码。但是这篇文章提到了对于monolithic service，是可以使用类似的pattern来设计代码的，清楚的区分开不同的模块，所以码农们可以各自专心修改自己的代码，而且这样子还减少了服务之间通信带来的overhead。

### Easier
很多application并不需要很严谨的划分出来各个模块，特别是像我司这种startup，需要最多的是prototyping，poc，所以需要很容易的去重新设计。这样子的话，micro service的确会是一个很好的选择，因为它可以很快的实现出来，不需要太多额外的dependency。但是这篇文章提到了一点就是首先你如果能提出这个服务用micro service的话，那其实你已经可以decouple成一个模块放在monolithic里面了，而且micro service带来的一个弊端是distributed transaction这个问题，这个一点也不简单。我个人比较赞同这个理由，但是前面那个理由不是特别有道理，因为在一个特别的大的code base上作修改实在是工作量太大了。之前在AWS实习时，一个简单feature的release要经过太多太多的审核和integration，导致开发周期特别长，但是带来的好处是系统十分稳定。

### Faster
文章的作者提出micro service并不会比monolithic快，因为performance的瓶颈一般不在CPU，而是在IO上，这样子的话micro service带来的额外的IO消耗太多了。而且他还提到了一点比较有意思的就是很多因为切换到了micro service从而提高了系统的performance，其实是因为切换了语言，从nodejs, ror换到了Scala和go。这一点貌似很有道理的样子，但是很难说。

### Simple
Micro service带来的趋势就是每个小组可以去负责一个简单的问题。但是这其实会带来很多其他的问题，作者提到的一个问题就是如果要做出一点点改动的话，你就需要把所有依赖的服务都跑起来；第二个问题就是很难写test，第三个问题是一个服务如果改动了但没有告知下游的服务的话，就会有大问题（我司经常见。。）。前两个我不是特别同意，因为这些都是可以mock出来的，特别是test。

### Scalability
作者并没有否决scalability，只是说了monolithic也是可以做到的，比如让monolithic有logical cluster的概念（。。。。）。感觉就是把monolithic拿来当micro service来用。。

## Takeaway
这篇文章总的来说还是说的不错的，只不过有几个点感觉是强行说micro的这个优点不成立。我的感觉micro service和monolithic像是两个极端，一个是极端分布式，一个是极端中心化。优点和缺点都很明显。极端分布式的缺点是很容易有outage，因为彼此都对对方的服务没有太多的context，所以很容易出错，好处就是真的可以scale。

## Reference
https://dzone.com/articles/microservices-please-dont?utm_source=wanqu.co&utm_campaign=Wanqu%20Daily&utm_medium=rss