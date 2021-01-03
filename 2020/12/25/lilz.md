# [Scaling Cache Infrastructure at Pinterest
](https://medium.com/pinterest-engineering/scaling-cache-infrastructure-at-pinterest-422d6d294ece)

- pinterest 服务调用链较长, 单个 api 请求带来的下游调用较多, 故对于服务调用延时会相对敏感, 单体服务的变慢或崩溃带来的雪崩效应强
- 分布式缓存在 pinterest 的使用场景主要在, 针对计算开销或存储开销较大的服务调用或数据库查询做缓存

## cache 和 cache proxy
选择了 memcached 和 mcrouter

选择 memcached 原因如下:

- 对水平扩展支持度很好, 高低负载时做伸缩方便
- 热数据读取高效(读时性能好)
- memcached 协议天然支持 TLS 协议, 对于做 pinterest 全链路 TLS 加密上在缓存层不会是阻碍
- 开源, 社区支持度高, 有持续迭代且 pinterest 也提供了很多 patch

选择 mcrouter 原因如下:

- 扩展性好, 控制面和数据面解耦, 数据面不仅仅支持 memcached 协议
- 鲁棒性高, api 模块独立
- 可观察性强, 常用指标做了打点
- 在协议层做了很多现代化特性的支持如 (TLS, 传输保温压缩等)

架构上, 非常的现代化.

mcrouter 以业务进程 sidecar 形式部署, 即每个业务进程拥有一个 mcrouter 进程做 memcached 层代理, 请求缓存时先打到 mcrouter, 再 proxy 到远端 memcached server 上, 其中 mcrouter 可以对请求进行多维度的负载均衡. 如可以对多地域的集群进行就近选则, 让请求打到就近的 mc 集群上. 可以对单集群按一定规则(取决于设计的 hash 算法)做均衡, 保障 mc 集群稳定性.

从隔离性角度, Mesh 角度都是一个非常现代化的设计.
即对于 mcrouter 做容器迁移时需要改造的成本小, 对于控制面数据面分离的设计很赞，1. 后续数据面切换易做兼容, 改造控制面代码和设计成本小, 或者可能根本无需调整. 2. 负载均衡等功能在控制面板做, 假设后续需要增加新的 memcached 服务发现/均衡, 无需修改数据面代码.

## Benchmark
for single memcahced instance

- 100K requests per second
- tens of thousands of concurrent TCP connections
- storage capacity from 55 GB to 1.7 TB

提了单机吞吐率, 连接数, 存储指标, 但没有提到时延相关指标.

## Scale

> One of the key features of a distributed system is horizontal scalability — the ability to scale out rather than scale up to accommodate additional traffic growth

这句话值得关注, 提到对于分布式系统应对流量增长, **scale out 比 scale up 来得更重要**.
我目前理解这个得依据场景, 对于分布式缓存来说确实更看重水平扩容.

由于 memcached 自身 arbitrarily scalable 的特性, pinterest 很容易可以对 memcached server 做水平扩容, 同时利用 mcrouter 在 lb 层追加新扩容的 server, 来保证请求抵达新扩容机器. 同时依赖于 mcrouter 提供一致性 hash 函数来保证同个 key 缓存分片的请求能尽可能落到同个 memcached server.

但上述为何要保证同个 key 尽量落到同个 mc server 这一点的背景在文章中没有提及, 查了下如下:

memcached 是带有 evict 的缓存系统, 可以简单理解为 LRU(只不过 mc 是环的数据结构), 对于热数据入环, 对于冷数据在环被填满时弹出.而对于请求量大的情况下, evict 频率会增加, 故保证一个后端同个数据热度很重要, 同个 key 落到同个 mc server 能尽可能做到这一点.

## pinterest 对于目前缓存架构上思考
其中提到两点我觉得比较有意思。

1. 对于 mcrouter 的升级、变更上如何做平滑升级降低风险.因为这样一个全局代理通常一发牵动全身, 一次变更通常作用于数千台机器, 如何做灰度就是一个比较重的问题.

> Leveraging a consistent hashing scheme for load distribution among a pool of upstream servers works well for the majority of cases, even if the keyspace is characterized by clusters of similarly-prefixed keys. However, this doesn’t solve problems with hot keys — an abnormal increase in request volume for a specific set of keys still results in load imbalance caused by hot shard(s) in the server cluster.

2. 这段话我理解了多遍. 即目前由于 mcrouter 提供的一致性 hash 来保证同 key/key-prefix 的缓存请求落到相同后端. 但这就破坏了负载均衡的规则, 即对于热 key 的突发流量可能会导致某个或某组 mc server 的负载突增, 导致 mc 集群负载不均衡的情况.*该如何解决呢?*
