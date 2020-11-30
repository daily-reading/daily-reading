**在选择缓存前，需要的问题：**

* 在系统中那些地方需要高吞吐、快速响应或者低延迟
* 如果使用缓存，数据一致性问题如何解决？
* 你要存储那种数据
* 你是否需要维护事务/主数据的缓存。
* 你是需要本地缓存还是共享缓存
* 你是否需要开源、商业或框架提供的解决方案
* 如果使用分布式缓存、那么性能、可靠性、可伸缩性和可用性如何呢？

**缓存带来的好处**

* 降低网络成本
* 提高响应
* 提升性能表现在同样的硬件上
* 网络中断期间内容的可用性

**常见的缓存使用方式**

* 本地内存
* 关系型数据库缓存
* Session 存储
* Token Caching
* Gaming
* Web page Caching
* Global  Id or Counter generation
* Fast Access To Any suitable Data

**缓存数据访问的策略**

* Read Through
* Write Through
* Write Behind Cachin
* Refresh Ahead Caching

**缓存淘汰策略**
* Least Recently Used
* Least Frequently Used
* Most Recently Used
* First In,First Out

**不同的数据类型缓存**

* Object Store
* Simple Key Value Store
* Native Data Structure Cache
* In-Memory Caching
* Static File Cache

**缓存策略**

* 进程内缓存
* 分布式缓存
    * Performance
    * Scalability
    * Availability
    * Manageability
    * Simplicity
    * Affordability

**现有的解决方案**

* Memcached
    * 缺乏持久性
    * 数据结构单一
    * 扩展机制不完善
    * 缺乏集群复制工具

* Redis
    * 不支持二级索引

* Aerospike
    * 支持混合存储（RAM+SSD 等）
    * 支持批处理、扫描、二级索引和聚合
    * 广告
    * 推荐引擎

* Hazelcast
    * 分布式存储
    * 自动化扩容

* Couchbase


**Cache-As-A-Service**

在使用缓存的过程中，不应该在服务中暴露具体的缓存实现，保留灵活性。

Memcached 还有什么用？


[All things caching- use cases, benefits, strategies, choosing a caching technology, exploring some popular products](https://medium.com/datadriveninvestor/all-things-caching-use-cases-benefits-strategies-choosing-a-caching-technology-exploring-fa6c1f2e93aa)
