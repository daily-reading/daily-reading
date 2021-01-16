# 租约
- 集群重的 node 需要对一些资源独占
  - 为什么不用锁
    - 持有锁的 node 可能会 crash
    - 或者是短暂的网络失联，进程暂停
    - 这些都导致锁不能被正常释放
    - 分布式锁实现还是有很多坑，有兴趣可以看这篇 [how to do distributed locking](https://martin.kleppmann.com/2016/02/08/how-to-do-distributed-locking.html)
      - 分布式中总会牵扯到 liveness 和 safety 的权衡问题。

## 租约的实现
- 租约是有时限，并且会失效，这就保证了不会出现无法释放的情况
- 租约还可以 renew ，如果想延长操作
- leader 负责跟踪租约的过期，并和所有 node 达成共识
- 看了一会，发现这个租约就是 raft 论文里面如何保证读一致性的方案
