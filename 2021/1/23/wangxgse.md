## 提交编号
NO.8@20210221

## 文章
[how-to-do-distributed-locking](https://martin.kleppmann.com/2016/02/08/how-to-do-distributed-locking.html)

这篇文章是Martin Kleppmann和 Redis作者antirez关于 分布式锁的一次争论。整个过程是：

Redis作者写了一篇文章：https://redis.io/topics/distlock , 讲述了如何用Redis实现分布式锁
Martin Kleppmann写了本文 [how-to-do-distributed-locking](https://martin.kleppmann.com/2016/02/08/how-to-do-distributed-locking.html)为作回应
Redis作者接着写了一篇文章 [Is Redlock safe](http://antirez.com/news/101)

## 读书笔记

### 作者认为用Redis作分布式锁不合适
> I think it’s a good fit in situations where you want to share some transient, approximate, fast-changing data between servers, and where it’s not a big deal if you occasionally lose that data for whatever reason.

> Redis has been gradually making inroads into areas of data management where there are stronger consistency and durability expectations – which worries me, because this is not what Redis is designed for. Arguably, distributed locking is one of those areas.

作者认为Redis非常适合用于临时的，快速变化的，非关键性的数据。但是现在越来越多的将Redis用于一些强一致性、有持久化期望的（我理解是更为关键性的数据），其中分布式锁就是其一。

### 分布式的两大场景分类
Efficiency 和 Correctness。如果仅仅为了Efficiency，那么分布式锁怎么简单怎么低成本怎么来，毕竟只是为了效率，就算多台机器都拿到了锁，只是影响了效率而已。
而如果是为了后者，出现两台机器都拿到了同一个资源的锁，会出现错误。

### Protecting a resource with a lock
分布式锁不像在单独的应用里面，会变得非常复杂，节点和网络以及其它因素总会出问题。
> It’s important to remember that a lock in a distributed system is not like a mutex in a multi-threaded application. It’s a more complicated beast, due to the problem that different nodes and the network can all fail independently in various ways.

~~~
function writeData(filename, data) {
    var lock = lockService.acquireLock(filename);
    if (!lock) {
        throw 'Failed to acquire lock';
    }

    try {
        var file = storage.readFile(filename);
        var updated = updateContents(file, data);
        storage.writeFile(filename, updated);
    } finally {
        lock.release();
    }
}
~~~

假如lockService实现是ok的，上述代码看似是ok的，但是程序会暂停，这个暂停可能会因为gc，IO、网络导致的延迟，网络丢包。
在程序暂停的情况下会导致两个节点都拿到了锁。

![](https://martin.kleppmann.com/2016/02/unsafe-lock.png)

为了解决这一问题，作者的建议是使用fencing token: a number that increases (e.g. incremented by the lock service) every time a client acquires the lock.
但是作者并没有解释这个token如何实现，个人觉得这个token的完美实现与分布式锁的难度也是不相上下。

![](https://martin.kleppmann.com/2016/02/fencing-tokens.png)

### RedLock的问题
- 没有token机制：it does not have any facility for generating fencing tokens.
- 时钟会跳跃，会导致锁会提前或者延迟失效 ：Thus, if the system clock is doing weird things, it could easily happen that the expiry of a key in Redis is much faster or much slower than expected.
- RedLock的安全性基于时间假设：Its safety depends on a lot of timing assumptions: it assumes that all Redis nodes hold keys for approximately the right length of time before expiring; that the network delay is small compared to the expiry duration; and that process pauses are much shorter than the expiry duration.

### 结论
RedLock : neither fish nor fowl
>  it is “neither fish nor fowl”: it is unnecessarily heavyweight and expensive for efficiency-optimization locks, but it is not sufficiently safe for situations in which correctness depends on the lock.

如果将锁用于正确性场景，不要用RedLock. 用更重的一致性方案。
> if you need locks for correctness, please don’t use Redlock. Instead, please use a proper consensus system such as ZooKeeper, probably via one of the Curator recipes that implements a lock. (At the very least, use a database with reasonable transactional guarantees.) And please enforce use of fencing tokens on all resource accesses under the lock.