###原文: https://martin.kleppmann.com/2016/02/08/how-to-do-distributed-locking.html



本文分析了Redlock作为分布式锁存在的问题

获取锁的两个理由
- Efficiency
  - 避免重复工作的开销
- Correctness
  - 避免并发带来的混乱

如果没有为了第一个目的，其实没必要去考虑使用Redlock.

####复习一下Redlock
Redlock是在集群Redis中的一个分布式锁的算法
通过集群避免单点失效。
提供
- 同时只会有一个client获取到锁
- 不会死锁
- Fault Tolerance. 只要大多数的Redis节点还活着，就可以正常运转

为什么failover-based的Redis锁解决方案不够呢？
因为主从架构的Redis，主从同步是异步的，极端情况下没法保证只有一个client获取到锁

Redlock procedures:
- 获取当前时间
- 依次向N个实例，用相同的key和随机值获取锁
- 客户端用当前时间 - 开始获取锁的时间 = 获取锁所消耗的时间。当从大多数节点都获取到锁 & 锁还未失效，才算获取成功
- 锁的有效时间 - 消耗时间 = 真正的有效时间
- 如果获取锁失败，广播解锁指令

####回到原文

列了一些加锁需要考虑的场景和反例
#####case1 gc过长导致锁被释放，导致写覆盖
![](https://martin.kleppmann.com/2016/02/unsafe-lock.png)

解决方法: token
![](https://martin.kleppmann.com/2016/02/fencing-tokens.png)

怎么在分布式环境中生成 fencing token? 
(zk可以用zid or znode version)

但是Redlock没有这个机制,又不能用一个redis节点来做（单点失效），也不能用redis集群来做（共识问题）

感觉：
感觉之前没有读懂分布式时钟的paper...
感觉后面可以继续读: http://antirez.com/news/101
