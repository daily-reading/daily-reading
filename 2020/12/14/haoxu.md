# Scaling Memcache at Facebook

* Author: Rajesh Nishtala, Hans Fugal, Steven Grimm, Marc Kwiatkowski, Herman Lee, Harry C. Li, Ryan McElroy, Mike Paleczny, Daniel Peek, Paul Saab, David Stafford, Tony Tung, Venkateshwaran Venkataramani
* Link: http://www.cs.utah.edu/~stutsman/cs6963/public/papers/memcached.pdf

overview:
1. instead of using updating cache after writing, delete it. This is because delete is idempotent.
2. generic cache. not only cache DB query result, but also pre computed result from ML algorithm. (Figure2,master-slave, lol, wonder why political correctness has no place in a liberal company like FB)
3. design goals: (1)No change if the issue is minor or has limited scope. (2) Relibility >> performance. OK to have stale data for a short period of time.

In a cluster:
Reducing latency:
issue: It's doing all-to-all communicatin between web servers and memcached servers, which could cause incast congestion or make a single server as a bottlenetck.
solution: focus on memcache client which runs on each web server.
1. parallel requests and batching: batch if possible with DAG, to reduce the number of requests for lower traffic.
2. client-server communication: clients are stateless, so we use UDP. If a packet dropped or out of order, treat it as error. Data supports it is practical. TCP via mcrouter.
3. Incast congestion:  sliding window to restrict the number of requests, which solves the issue. Data suggests ~300 as the reasonable window size

Reducing load:
1. Leases: servers return a token per key per 10 secs. So for stale sets, the token would be invalidated at the time of trying to write in cache (Q: how? Need to read the reference papaer). For thundering herd, with the 10s restriction, the read action would be deferred for a few millisecs wihch avoids the issue. And the deleted key/value would be saved in a data structure, which would be flushed all together repeatly on schedule. This returns data which is stale but is acceptable considering the perf benifit it brings.
2. Memcache pools: Divide memcache servers by key access frequency.
3. Replication within pools: Replicate memcache servers within pools in specific cases.

Handling Failures:
1. Gutter (lol, I just have my gutters cleaned recently) servers as backup servers in case a small amount of memcache servers are down. Gutter server entries expire quickly since they are just backups assuming original memcache servers should be back online quickly.
2. widespread outage: if Gutters cannot handle, and the cluster has to be taken offline, just redirect to other clusters.

In region: Replication:
1. Regional Invalidations
each database has invalidation daemons, which are used to broadcasts these deletes to the memcache in every frontend clusters.

Q: It says only 4% of all deletes issued result in the actual invalidation of cached data. What does this mean? only 4% of all deletes are related to cache? How about update operation which also triggers a cache invalidation?

(1 Reducing packet rates: batch
(2 Invalidation via web servers: 2 issues for broadcasting via web servers. do it via mcsqueal avoids those issues and has a replay bounus.

2. Regional Pools
multiple frontend servers share the same memcached servers -> regional pools.
For less frequently accessed category, e.g.  kind B in this case, convert it to a shared regional pools insted of being replicated per region.

3. cold cluster warmup
warm up a cold cluster by a warm cluster.
pro: bring a cluster back to full capacity within hours instead of days. Good for disaster recovery.
con: potential race condition. Solution: delay delete by 2s. Race condition is still possible, but acceptable and fixable.

Across Regions: Consistency:
main issue: replicate dbs lag behind master db
replicate dbs are readonly.
1. Writes from a master region
mcsqueal rocks!

2. Writes from a non-master region
remote marker mechanism: set a remote marker for directing the query to master region to lower the probablity of reading stale data. ("Stupid" but works)

3. Operational considerations
similar to mcsqueal, e.g. batching and replaying.

Single Server Improvements:
1. Performance Optimizations
    1.1 automatic hash table expansion
    1.2 multi thread using global lock
    1.3 each thread has its own UDP port

2. Adaptive Slab Allocator
LRU and memory management improvement

3. The Transient Item Cache
Similar to leases, leave expired items into a separate data strtucture.

4. software updates
software updates may take a server down. So store the memcached values and main data structure in system V shared memory regions, so the data can live across a software upgrade.

Q: what is the difference between this and warmup a cold cluster? What's the benefits?

Memcache Workload:
Experiences and data analysis

Related Work:
many similar tools and platforms, like DeCandia by Amazon, which is focused on wirte heavy worload. But memcache is focused on reads. 
Linkedin uses Voldemort. Otherwis like Redis, memcached, Cassandra etc.

Conclusion:
1. Separating cache and persistent storage systems allows us to independently scale them.
2. Features that improve monitoring, debugging and operational efficiency are as important as performance. (yes, I am talking to you, ES team)
3. Managing stateful components is operationally more complex than stateless ones.
4. The system must support gradual rollout and rollback of new features even if it leads to temporary heterogeneity of feature sets
5. Simplicity is vital. (claps.....)

My words:
这周想challenge一下自己，读篇论文吧，但发现读论文水平下降太多了（上次读论文大概10年前？）。大概high level的了解了下memcache的架构和各种设计的取舍，但如果要详细的了解implementation，要把reference好好读一遍才行。