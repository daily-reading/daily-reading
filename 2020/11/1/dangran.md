https://medium.com/box-tech-blog/strategies-used-at-box-to-protect-mysql-at-scale-35388b85f2a4

Data access infrastructureure
Guarantee:
* Strict data consistency
    * 一致性保障
* High throughput
    * client端的数据吞吐能力保障。作为data access，应该算是无上限的吞吐
* Low latency
    * 接入层应该很低的延迟，毫秒级
* High avalablilty
    * 最高级别的可用性

Adding a data access service
因为连接池资源不足，尝试构建 distributed data-access service
一致性问题：因为只有少数的请求能够尝试路由到从。
我们的目标是 从从库读取一致性的数据。
常规做法是去check 是否同步到从，如果是则读，不是则做额外的处理
First Option: fail the request。大多情况可用，但是在延迟较高的情况下，则会大量失败
Second Option: 降级到主. 但这是一个富有欺骗性的坏主意。因为会导致雪崩。

我们实现了：写后读的一致性保障。


蓝色为写，则从从库读取，在t3后才能保证写后读的一致性。[扩展阅读<How We Learned to Stop Worrying and Read from Replicas>: https://medium.com/box-tech-blog/how-we-learned-to-stop-worrying-and-read-from-replicas-58cc43973638]
总结：
* 牺牲部分延迟，保证写后读一致性，来路由大部分请求到从库
* 大多数的读不需要去牺牲延迟，因为不存在一致性问题
​
第二部分：improving cache utilization
只缓存 主库读取的数据，因为从库读取数据的缓存极为复杂。
随着多从，最后缓存的越来越少。
为了应对这种情况，设定了几个策略
* 缓存 已同步主库的从库读取的数据
* 缓存 不存在的对象的存在性
    * ​​缓存一个特殊值来标记此对象不存在。
    * 能提高10%的缓存命中率
* 缓存租约？（不知道中文叫啥） cache lease (扩展阅读: http://www.cs.utah.edu/~stutsman/cs6963/public/papers/memcached.pdf. https://medium.com/box-tech-blog/cache-is-the-root-of-all-evil-e64ebd7cbd3b)
    * 部分值是热点值，高频读写
    * 可能导致惊群问题(thundering herds)
    * 只有Lease Holder 可以修改值
    * 其他读取要等待修改完成

第三部分：限流 rate limiting 

需求
* Decentralized information propagation
    * 数据热点均匀分布
* Fault tolerant 错误容忍
    * 自动恢复
* Fast convergence - 快速聚合
    * 因为数据分散，跨节点的检索需要快速聚合

扩展阅读: https://www.cs.cornell.edu/johannes/papers/2003/focs2003-gossip.pdf

第四部分
以上三个部分都是扩展读能力，那么写呢？
异步写越来越多

1 允许client标记写的质量需求（优先级？）
有效防止低优先级请求把服务打降级
提供feedback loop to client，使得client可以控制自己的速率
削峰

新增了一个 medium QoS : customer-triggered asynchronous traffic
在关键时刻可以降级 low pr的写

