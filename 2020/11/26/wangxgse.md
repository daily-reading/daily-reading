## 提交编号
NO.1@20210102

## 文章 https://medium.com/datadriveninvestor/all-things-caching-use-cases-benefits-strategies-choosing-a-caching-technology-exploring-fa6c1f2e93aa

## 总体介绍
这篇文章介绍了缓存能够解决哪些问题，缓存应用的一些最佳实践：即缓存策略，以及现有市面上的缓存解决方案。

## Caching Prerequisites
* 引入缓存是为了解决存在的问题，同时也带来了新的问题，新的问题主要是指引入缓存带来的数据一致性问题。
* 业务侧：你要缓存什么数据，对缓存容忍度如何。
* 缓存侧：本地缓存、远程缓存（单机 or 分布式），如何是分布式，缓存系统的可靠性、可扩展、可用性是什么样的。

## Caching Benefits
* 文中的前三点都是性能问题，第四点（Availability of content during network interruptions）是提升了系统的可用性

## General Cache Use Cases
* In-memory data lookup
* RDBMS Speedup
* Manage Spike in web/mobile apps (处理系统的峰值)
* Session Store
* Token Caching
* Gaming
* Web Page Caching
* Global Id or Counter generation
* Fast Access To Any Suitable Data

## Caching Data Access Strategies
缓存策略在coolshell有篇文章讲的更清楚 【缓存更新的套路: https://coolshell.cn/articles/17416.html】
* Read Through / Lazy Loading
* Write Through
* Write Behind Caching
* Refresh Ahead Caching
文章没有讲的是 cache aside。

## Eviction Policy
* Least Recently Used (LRU)
* Least Frequently Used (LFU)
* Most Recently Used (MRU)
* First In, First Out (FIFO)
日常听到最多是LRU，LFU,MRU 很有意思。这也说明，任何策略的选择要基于你当前的场景。

## Data Type Wise Cache
## Caching Strategy

## Real Life Caching Solutions
* Memcached
* Redis
* Aerospike
* Hazelcast
* Couchbase

## The Last Thing: Cache-As-A-Service
缓存作为一个服务，这个与引入其它组件是类似的道理。

