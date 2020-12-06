**数据访问层特性**:
1. 严格的数据一致性
2. 高吞吐
3. 低延迟
4. 高可用

**如何提升副本的利用率？**
如何将大量写后读的请求转移到读库上？
转移到读库的时候如何保证读到新的数据？
是不是一定需要最新的数据？
每个请求不一定需要最新的数据，他只需要触发他读的这个写请求的数据。
**如何提高缓存利用率？**

* 缓存不存在的对象，防止查询请求打到 DB
* 使用租约，在缓存未命中时，进行缓存填充

**限流**
通过 gossip 协议实现，没太看懂。
关于 Gossip 协议:https://www.jianshu.com/p/54eab117e6ae

**降级策略**
背压、流量标记区分优先级


How We Learned to Stop Worrying and Read from Replicas
缓存是万恶之源
[Strategies Used at Box to Protect MySQL at Scale](https://medium.com/box-tech-blog/strategies-used-at-box-to-protect-mysql-at-scale-35388b85f2a4)
**这里数据访问层的职责到底是什么？**

