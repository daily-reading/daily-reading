# Scaling Memcache at Facebook

This [paper](http://www.cs.utah.edu/~stutsman/cs6963/public/papers/memcached.pdf) introduces how Facebook managed to construct and scale a distributed k-v store leveraging memcached.

## Overview
Memcache, as the name indicates, is a in-memory cache solution which provides a simple set of data manipulation operations like set, get and delete. With this building block, Facebook made it more efficient by tuning it into an easy-to-scale distributed k-v store and decorating it with features like auto configuration and fault tolerance. Let's call the system memcache, for simplicity.
In the rest of the paper, the authors start from the original single cluster version of their memcache solution. When single cluster failed to meet the growing requests,replicas of the principle cluster emerges. After that, cross-region consistency problem gets evaluated. Finally, performance tuning and corresponding statistics are given.

## In a Cluster: Latency and Load
### Reducing Latency
Memcache reduce latency mainly by focusing on the client side. In this way, the memcache server remains stateless and become easier to scale. Memcache client do two major work:
* **Parallel requests and batching**. Batch the requests that have no dependencies between them to reduce network round trip time.
* **Client-server communication**. Provides routing abilities. For get requests, UDP is used and for requests that requires reliability, TCP is a better choice.
* **Limit incast congestion**.  Memcache clients implement flow-control mechanisms, somehow like the TCP sliding window. The window size is carefully tuned.

### Reducing Load
The mission of cache is to prevent request from falling back to expensive paths like databases queries or run-time computations as much as possible.
* **Lease**. Bound a token to the specific key under cache manipulations. Out-dated tokens will not trigger next round operations thus protecting the system from heavy read and write requests on a hot key. By limiting the distributing rate of tokens, thundering herds also get mitigated. Moreover, clients will be able to be informed of the existence of stale values. If they feel free to use those values, no further waiting or retry is needed.
* **Memcache pools**.  Memcache separate low-churn key families and high-churn ones into distinct pools, as their quality-of-service requirements and cache miss cost are dramatically different. For hot keys, replicas are introduced to make life easier, for memcache servers as well as clients.
* **Handling Failures**.  Memcache appoint 1% of the machines in the cluster as *Gutters*. Under outages, clients shunt load to these idle memcache servers.

## In a Region: Replication
'Memcache trade replication of data for more independent failure domains, tractable network configuration, and a reduction of incast congestion'.
* **Regional invalidation**. Replicas get synced by inspecting the databases operations, binlog for example. In this way, sync commands are packed and then send to individual memcache servers. Besides, invalidation operations logged in databases are much easier to replay comparing with http requests launched by frontend clusters.
* **Regional pools**. Multiple frontend clusters can share the same set of memcache servers, which is called a regional pool. This strategy is especially useful for keys with low access rate. To be specific, the decision to migrate keys into regional pools is based on access rates, data set size and number of unique users accessing them.
* **Cold Cluster Warmup**. When booting, the cold cluster is allowed to steal data from the already warm ones. Some minor data inconsistency might happen during the warm up, but that doesn't hurt.

## Across Regions: Consistency
For Facebook, it seems that performance weights more than consistency :) Let's just highlight the interesting part.
* **Remote marker** mechanism is deployed to minimize the probability of reading stale data. By leaving such markers within the replica, writes from a non-master region manages to inform the later comings that a modification is on the flight. A decreased chance of data inconsistency is gained at the cost of additional latency in case of a cache miss.

## Single Server Improvements
### Performance Optimizations
* Automatic expansion of the hash table.
* Multi-threaded server with fine-grained data lock.
* Give each thread its own UDP port to reduce contention.
### Adaptive Slab Allocator
The algorithm trigger a slab size increase when the age of the recently evicted item is at least 20% more recent than past. Seems like the memory chunk size never gets back down.
### The Transient Item Cache
Upon the default lazily evicts of entries, short-lived items are put into a circular buffer. At the end of each second, all the items in the head of the buffer are evicted and the head moves on.

## Memcache Workload
Omitted.

## Related Work
* Amazon has their own k-v store optimized for a write heavy workload.
* Github leverage Redis as caching systems.
* Cassandra, a schema-based distributed k-v store, is used at Lakshman *et al.*.

## Conclusion
Great paper. Simple design and appropriate trade-offs. The practice of batching the requests and keep the service stateless are quite universal.