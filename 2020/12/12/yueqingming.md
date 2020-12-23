### Application Data Caching
![2b5b56e2cdd4fd1446d22add3f010332.png](evernotecid://7F01B206-A616-4EA4-AABF-0C5FA9B75355/appyinxiangcom/26825047/ENNote/p189?hash=2b5b56e2cdd4fd1446d22add3f010332)
### The Backbone of Distributed Caching: Memcached and Mcrouter
Mcrouter 是 Memcached 的一个七层代理
Memcached 的特性：

* 异步事件驱动体系和多线程处理模型，高性能，水平扩展
* Exstore 通过内存与 NVMe flash 结合提升存储容量（可以从 55GB 提升到 1.7TB）
* Memcached 简化架构，单个节点之间不知道集群的概念
* 社区活跃，性能和准确性经过几十年的验证
* Memecached 支持 TLS 认证

为什么选择 Mcrouter:

* Mcrouter 为 Memcached 提供一个统一的接入点，保证 Memcached 集群的负载均衡
* 解耦控制平面和数据平面
* Mcrouter 提供了区域关联路由，数据冗余复制，多级缓存层和影子流量等功能。
* Mcrouter 提供协议控制
* 丰富的可观测指标
![dcca4011ae1e237e4ade1d945c2bd9dd.png](evernotecid://7F01B206-A616-4EA4-AABF-0C5FA9B75355/appyinxiangcom/26825047/ENNote/p189?hash=dcca4011ae1e237e4ade1d945c2bd9dd)

### Compute and Storage Efficiency
单个 r5.2xlarge EC2 实例（具体的配置）：

* 100K QPS
* 数万个并发 TCP 连接

### High Availability

* 自动故障转移
* 通过透明的跨区域复制实现数据冗余
* 针对实际生产流量进行独立影子测试

### Load Balancing and Data Sharding
一致性哈希

### Tradeoffs and Considerations

* 中间层会增加 IO 和计算的开销，但是中间层提供了一些关键的功能（构建 Memcached 集群，路由等）
* 全局同步配置变更，影响面较大，但是也保证了全局路由策略一致。
* 维护了 100 多个集群，有着不同的硬件和路由策略，给团队带来了运维负担，但是也给业务团队提供了选择的机会。
* 大多数情况下一致性哈希可以正常的工作，但是依然没有解决热 Key 的问题。

### Future Work

尝试将 Memcached 内嵌到应用程序中，减少 IO 的开销。（这不就成了本地缓存？）



