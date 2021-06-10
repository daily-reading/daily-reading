Caching Prerequisites
Note: some questions you should ask yourself before applying cache, e.g. where to apply cache and ok with data inconsistency?

Cacheing benefits
Note: short answer, fast, low cost, better perf. And in some cases, availability in case of outage.

use cases
Note: in memory data loolup etc. to summarize, any expensive operations "should" be cached for better perf.

Caching Data Access Strategies
1. Read Through / Lazy Loading
just read and lazily load the data. good for cases where not all data needs to be cached

pro: only load partial data. good for decouple and modulization. And in case of cache miss, it keeps populating required data.
con: cache miss causes delay, and stale data could be an issue.
Note: one more thing it doesnt mention. Lazy loading also has a cost. In case it lazy loads too much external data, it might cause a delay anyway.

2. Write Through
invalidating cache and update it. good for cases where stale data is not acceptable

pro: no stale data.
con: write might be expensive, cache churn and in case of cache failure, entire transaction fails

3. Write Behind Caching
update cache first and deferred write to data source. good for high read/write system

pro: isolated from the underlying data source.
con: potential data inconsistency

design constraints:
1. roll back required in case failed to write to data source
2. no foreign key constrain

4. refresh ahead caching
refresh cache before it expires

pro:good for large number of users.
con: implementation

Eviction Policy
This reminds me of LRU (lol, just saw it below).

LRU
pro: most optimal
con: mild success rate since frequency is more important

LFU
pro:
1. more nature
2. good for cases under high load and a lot of elements is requested.
con:
1. hight frequent used item will only be evicted after a long time
2. implementation

MRU:
FIFO:

Data Type Wise Cache
1. Object Store:
2. Simple Key Value Store:
3. Native Data Structure Cache:
4. In-Memory Caching: 
5. Static File Cache:

Caching Strategy
1. single node. 
not distributed
pro: local data, hight speed, easy to maintain
con: high memory consumption
use cases: app, web frontetn end app for caching website data like images, css, etc.

2. distributed caching
requirements
1. perf
2. scalability
3. availability
4. manageability
5. simplicity
6. affordability

Real Life Caching Solutions
Memcached
https://memcached.org/

redis
https://redis.io/

Aerospike
Hazelcast

Cache-As-A-Service