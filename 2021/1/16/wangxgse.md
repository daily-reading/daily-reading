## 提交编号
NO.6@20210206

## 文章
https://martinfowler.com/articles/patterns-of-distributed-systems/time-bound-lease.html

## 读书笔记

### 面临的问题
分布式中面试的一个问题是，分布式节点需要对一些资源进行排他进访问，这种情况下通常做法是加锁，但是加锁需要考虑重入、存活等问题。
而节点可能会crash，可能会抖动，这种场景下会会出现节点对于这个资源拥有无限期的访问权限。为解决这一问题，在分布式领域，引入了
Lease的概念。Lease的概念的首次提出是在1989年，[Leases@1989](https://web.stanford.edu/class/cs240/readings/89-leases.pdf)

### 解决方案
- Lease:
  - 在一定时间内对资源进行排他性访问的权限。
  - 发起创建Lease的client有责任对Lease进行续约
    - It's reasonable to send a request after about half of the lease time is elapsed
    - The client node tracks the time with its own monotonic clock.
  - 如果client不续约，到期由自动过期。
    - > When the lease expires, it is removed from the leader. It's also critical for this information to be committed to the Consistent Core. So the leader sends a request to expire the lease, which is handled like other requests in Consistent Core. Once the High-Water Mark reaches the proposed expire lease request, it's removed from all the followers.
  - Lease在主从之间像普通数据一样进行备份，但是只有主节点负责对Lease进行过期操作，当主将Lease过期时，会将信息同步到从节点
    -  需要保证对一同一份资源（或者说标的），Lease是唯一的。
    -  和其它写请求一样，只有租约到达水位时才表示成功(The request is complete only when the High-Water Mark reaches the log index of the request entry in the replicated log.)

### Attaching the lease to keys in the key value storage
- 将lease 和 kv值绑定。
  - 个人理解：kv可以多个，即同一个lease可以绑定多个kv，类似zk中的session和临时节点的关系
    - > zookeeper has a concept of sessions and ephemeral nodes. The sessions are implemented with the similar mechanism explained in this pattern. Ephemeral nodes are attached to the session. Once the session expires, all the ephemeral nodes are removed from the storage.

### Handling leader failure
- 主如果挂了之后，从中选出的新的主会将所有的lease重新续约
  - > The new leader refreshes all the leases it knows about. Note that the leases which were about to expire on the old leader get extended by the 'time to live' value. This is not a problem, as it gives the chance for the client to reconnect with the new leader and continue the lease.

### 文中提到的其它概念
- [consistent-core](https://martinfowler.com/articles/patterns-of-distributed-systems/consistent-core.html)
- [high-watermark](https://martinfowler.com/articles/patterns-of-distributed-systems/high-watermark.html)

- Wall Clocks are not monotonic:
  - > All programming languages have an api to read both the wall clock and the monotonic clock. e.g. In Java System.currentMillis gives wall clock time and System.nanoTime gives monotonic clock time.
