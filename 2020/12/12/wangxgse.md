## 提交编号
NO.5@20210131

## 文章
https://medium.com/pinterest-engineering/scaling-cache-infrastructure-at-pinterest-422d6d294ece

## 读书笔记

### Application Data Caching
> Offering a distributed cache tier as a service allows application developers to focus on implementing business logic without worrying about distributed data consistency, high availability, or memory capacity. Cache clients use a universal routing abstraction layer that ensures applications have a fault-tolerant and consistent view of data.

一个分布式缓存tier，让研发人员能够聚集于业务逻辑，而不是缓存细节。

### The Backbone of Distributed Caching: Memcached and Mcrouter

pinterest缓存基础设施两大核心：Memcached 和 Mcrouter
Mcrouter是 facebook的一篇论文 《Scaling Memcache at Facebook》 有过论述，而且已经开源出来了：https://github.com/facebook/mcrouter

Mcrouter is a layer 7 memcached protocol proxy that sits in front of the memcached fleet and provides powerful high availability and routing features.

- Mcrouter acts as an effective abstraction of the entire memcached server fleet by providing application developers a single endpoint for interacting with the entire cache fleet
- Mcrouter presents a decoupled control plane and data plane
- Mcrouter’s configuration API provides robust building blocks for complex routing behaviors
- mcrouter exposes intelligent protocol-specific features like request manipulation (TTL modification, in-flight compression, and more).
- Rich observability features are provided out of the box at no cost to the client application, providing detailed visibility into memcached traffic across all of our infrastructure

mcrouter本质上是一个"客户端"，让业务方不用直连memcache，这个客户端的部署方式在pinterest经历了如下演进
In particular, routing and discovery was first done in client-side libraries (which was brittle and coupled with binary deploys), followed by an in-house built routing proxy (which didn’t provide extensible building blocks for high availability), and finally mcrouter.

### Compute and Storage Efficiency

在Pinterest, memcached的 extstore 机制被应用，extstore(https://github.com/memcached/memcached/wiki/Extstore)是什么：
> Extstore is an addition to memcached which leaves the hash table and keys in memory, but moves values to external storage (usually flash).

### High Availability
* Automatic failover for partially degraded or completely offline servers.
* Data redundancy through transparent cross-zone replication.
* Isolated shadow testing against real production traffic.

### Load Balancing and Data Sharding
一致性hash

### Tradeoffs and Considerations
* 基于proxy会带来一些性能上的消耗，但是其带来其它方面的收益是很巨大的。（high availability abstractions, flexible routing behaviors）
* proxy配置全局统一，带来了风险的同时也有收益
* 运维团队负责近100个集群，每个都不太一样。带来运维压力的同时，也带来收益，收益比如：更好的隔离，更切合各种业务场景。
* 一致性hash很好用，但是对于hot key也是无能为力。
