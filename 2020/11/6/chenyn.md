# [How We Learned to Stop Worrying and Read from Replicas](https://medium.com/box-tech-blog/how-we-learned-to-stop-worrying-and-read-from-replicas-58cc43973638)

- You realize that you inadvertently stepped into the mucky marshlands of eventual consistency（ 这句话第一眼看得我有点懵逼 ）
- 对于主从一致性问题，作者提到如遇到 replication-lag-sensitive but latency-tolerant（ 我理解为「 一致性优先 」？）的场景时，会采用轮询是否能在窗口期内完成 replicate 的方式保证一致性，但代价是 high latency，否则就 timeout 返回约定的错误状态。但通常情况下 retry 在压力没有减小时完全没用
- 另一个思路是可以不等到 slave 结束同步后再读，而是可以在写时记录对应的 position，读时只要发现 slave 上这个 position 已经同步就可以放心读了，即使整个 database replication 会有 lag 也没关系。这个 position 和资源的关系需要映射到 data access layer 做记录