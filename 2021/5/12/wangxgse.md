# 提交说明
- 编号：NO.21@20210512
- 文章：[Handling Distributed Transactions in the Microservice world
](https://medium.com/swlh/handling-transactions-in-the-microservice-world-c77b275813e0)

# 读书笔记
- 这篇文章读之后最大的收获是看到了文中引用的一篇论文[Life beyond Distributed Transactions](https://www.ics.uci.edu/~cs223/papers/cidr07p15.pdf)，简单搜索了一下，我们熟知的TCC来源于此篇论文，有时间把这篇论文给看了。
- 不管是2PC还是 Eventual Consistency and Compensation(SAGA) 都是一种思想，完整实现一套的难度大概跟分布式锁是一样的。但是在面对实际问题的时候，基于当时的场景，对这些思想进行剪接，对于解决我们自己的实际问题是有裨益的。