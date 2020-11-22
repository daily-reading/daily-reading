[Article](https://medium.com/box-tech-blog/cache-is-the-root-of-all-evil-e64ebd7cbd3b) Cache is the Root of All Evil

### Aim

1. To lower latencies.
2. To ligthen the system load.

*Howerver, introducing caching inevitably introduces correctness problems, that is hard to debug and escalates the system chaos by adding an extra layer*

Advice: If not necessary, DO NOT use cache :)

### Update on read

It is easy to understand how a cache works to optimize latency and load, especially in a read-heavy case. 

**Just to clarify, we are taking about a cache implementing the following rules:**

1. When read, first look up vaule in cache.
2. If read-hit, return value.
3. if read-miss, read from system of record & update cache.
4. When write, write to system of record & invalidate the corressponding slot in cache.

Differente from both write-back and write-through, where changes are introduced into cache with a write-event, the above mechanism populates change to cache with a read-event. 

#### Advantages

Though, It could be hard to find which corresponding value in the cache to invalidate when a write happens. It still has some prominent advantages comparing the traditional two mechanism:

1. Reduced operation complexity, that has 2 optional paths with read and only 1 path with write.
2. Relatively easily reproducible and testable
3. Less severe concurrency-related cache consistency problems

#### Drawbacks

Of course, there not exists a silver bullet that could solve all evil side of caching. Each mechanism has its own speciall features and troubles for granted.

1. In case of high-volume read traffic, a write with a cache value .invalidation can lead to a **thundering herd (惊群)** of readers storming the system of record to reload the value into cache.
2. A concurrent read-miss and write can cause a stale value to be stored in cache indefinitely. .

**Solution :** "Lease" mark as a per-cache-ky lock to prevent thundering herds and stale sets. By altering the previously mentioned cache read-write mechanism to the following, we can have the above two troubles solved.

1. When read, first look up vaule in cache.
2. If read-hit, check whether the key has a lease mark with it.
   - It not, return value.
   - If true, sleep a while and re-do read. (Someone might be updateing the value of the key concurrently).
3. if read-miss, try to add a lease to the key.
   - If key already has a lease, go #2 to do read.
   - Key now has a lease (only one request can do this amoung all concurrent multi-requests). Read from system of record & **recheck the lease** (to prevent super-high read latency caused by CAS and interruptions from writes, when data is consistently read frequently with periodic bursts of frequent writes).
     - CAS check succeed: update cache & cancel the lease & return the value.
     - CAS check failed (becasue writer invalidate the affected key): Lease get cleared & save the retrieved value with lease somewhere else. Any other reader that is waiting for the this lease could **get the value retreived by the lease hould safely**, because these read requests arrives before the write request being accomplished.
4. When write, write to system of record & invalidate the corressponding slot in cache & acknowledge the write event.
