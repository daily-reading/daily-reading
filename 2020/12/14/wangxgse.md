## 提交编号
NO.3@20210116

## 论文 http://www.cs.utah.edu/~stutsman/cs6963/public/papers/memcached.pdf

## 总体感受
这是一篇在很久之前就知道，一直想读，一直没有读的文章。感谢这个读书的机制，让自己能把这个给读了。

这篇文中看到warm up机制, 看到的缓存读写模式（demand-filled look-aside cache），看到的McSqueal机制，看到的thundering herds（惊群效应），看到的stale data，lease(租约) 。很多概念在之前都知道，但是并不知道是从哪儿来的，读完这篇有种看到源头的感觉。想到了中学时代老师说读书破万卷，下笔如有神的话。

以上是总体感觉，下面是具体的读书笔记。

## In a Cluster: Latency and Load

> At this scale, most of our efforts focus on reducing either the latency of fetching cached data or the load imposed due to a cache miss.

### Reducing Latency
- We reduce latency mainly by focusing on the memcache client, which runs on each web server. This client serves a range of functions, including serializa- tion, compression, request routing, error handling, and request batching. Clients maintain a map of all available servers, which is updated through an auxiliary configu- ration system.
- 解决延迟问题的主要手段集中在客户端，依文中所述，facebook的 memcache client是个很重的客户端，即"胖"客户端：包括压缩，路由，错误处理，批处理，路由表管理。
    - Parallel requests and batching：用DAG表示数据之间的关系
    - Client-server communication
        - 两种client模式：library&proxy: Client logic is provided as two components: a library that can be embedded into applications or as a standalone proxy named mcrouter. This proxy presents a memcached server interface and routes the requests/replies to/from other servers.
        - TCP/UDP: rely on UDP for get requests to reduce latency and overhead. / For reliability, clients perform set and delete opera- tions over TCP
        - Incast congestion：Similar to TCP’s congestion control, the size of this sliding win- dow grows slowly upon a successful request and shrinks when a request goes unanswered. The window applies to all memcache requests independently of destination
### Reducing Load
> We introduce a new mechanism we call leases to address two problems: stale sets and thundering herds. 

- lease：租约
memcache在接收到client请求时，如果没有数据，除了返回给客户端miss之外，还返回一个token，client会在db取数据，取到之后，会再存储进缓存，这一存储请求会带着之前拿到的token，缓存会通过token进行验证，是否允许此次更新。
这一策略通过更改返回token的频率，来降低惊群效应，降低对于数据库的瞬时负载。此外memcache会把要过期的数据放进一个临时的结构，返回给客户数据，并标记为过期，如果业务上可以容易可以直接使用。

- Memcache Pools : 
    - 针对不同的场景，设立不同的pool。场景的维度：miss的容忍度，refill缓存的成本，数据量的大小，变化的频率等等。
    - 在pool之间进行数据复制来优化性能

## In a Region: Replication
> It is tempting to buy more web and memcached servers to scale a cluster as demand increases. However, na ̈ıvely scaling the system does not eliminate all problems. Highly requested items will only become more popular as more web servers are added to cope with increased user traffic. Incast congestion also worsens as the num- ber of memcached servers increases. We therefore split our web and memcached servers into multiple frontend clusters. These clusters, along with a storage cluster that contain the databases, define a region.

提高性能如果只是单纯的加机器是不行的，机器越多，web server就需要跟越多的机器建立连接，同一个请求就需要从更多的机器上获取数据，网络拥塞严重；一些热Key无论你如何加节点依旧是存在的。因此从加节点变为再加一个cluster，多个cluser构成了region（We therefore split our web and memcached servers into multiple frontend clusters. These clusters, along with a storage cluster that contain the databases, define a region）

- 同步数据问题 
基于binlog实现一处更新同步至所有cluster的问题。binlog -> McSqueal -> Mcrouter -> memcache
- Regional Pools
提供一套公用的pool，供所有集群使用，至于什么数据可以迁移至公用pool，依赖于人的经验。（The decision to migrate data into regional pools is currently based on a set of manual heuristics based on access rates, data set size, and number of unique users accessing particular items.）
- Cold Cluster Warmup
> A system called Cold Clus- ter Warmup mitigates this by allowing clients in the “cold cluster” (i.e. the frontend cluster that has an empty cache) to retrieve data from the “warm cluster” (i.e. a cluster that has caches with normal hit rates) rather than the persistent storage. 

cold集群在cache miss的时候，从hot cluster去拿数据，这种机制可以快速的将新加的集群warm up。这个机制存在如下问题：
cold集群删除了一个数据，这个数据会同步到其它的集群，在尚未完成的时候，cold集群会来读这个数据，发现没有，就去warm数据去读取，这个数据就会被缓存在cold集群，而这个数据其实是被cold集群删除的。
为解决上述问题，论文给出的方案是hold-off time，即在delete之后，一段时间不允许add.

## Across Regions: Consistency
> Each region consists of a storage cluster and several frontend clusters. We designate one region to hold the master databases and the other regions to contain read-only replicas; we rely on MySQL’s replication mechanism to keep replica databases up-to-date with their masters. 

These challenges stem from a single problem: replica databases may lag behind the master database.

## Single Server Improvements

- Performance Optimizations
    -  allow automatic expansion of the hash ta- ble to avoid look-up times drifting to O(n)
    - make the server multi-threaded using a global lock to protect mul- tiple data structures
    - giving each thread its own UDP port to reduce contention when sending replies and later spreading interrupt processing overhead.
- Adaptive Slab Allocator
- The Transient Item Cache
- Software Upgrades

## Conclusion
- lessons learned : While building, maintaining, and evolving our system we have learned the following lessons.
    - Separating cache and persistent storage systems allows us to independently scale them. 
    - Features that improve monitoring, debugging and operational efficiency are as important as performance. 
    - Managing stateful components is operationally more complex than stateless ones. As a result keeping logic in a stateless client helps iterate on features and minimize disruption. 
    - The system must support gradual rollout and rollback of new features even if it leads to temporary heterogeneity of feature sets. 
    - Simplicity is vital.