## 提交编号
NO.4@20210124

## 文章
https://medium.com/box-tech-blog/cache-is-the-root-of-all-evil-e64ebd7cbd3b

## 读书笔记

1. denormalization
    > It is almost a law of nature that once you introduce a denormalization, it’s a matter of time before it diverges from the source of truth.

    > 来自wikipedia Denormalization is a strategy used on a previously-normalized database to increase performance. In computing, denormalization is the process of trying to improve the read performance of a database, at the expense of losing some write performance, by adding redundant copies of data or by grouping data

    与source of truth不一致只是时间问题，这儿用了个词语 denormalization , 最后搜到了 https://en.wikipedia.org/wiki/Denormalization ，里面有句话解释了denormalization。反范式的范式即关系型数据库的范式，仔细想想，加了缓存，从范式角度确实是反了范式，即反范式

2. 缓存策略

    如下图所示，这种缓存策略是最常见的，但是命名好像都不太样，facebook的论文中称之为（demand-filled look-aside cache），也见过叫 cache-aside cache名字的。
    ![](https://miro.medium.com/max/700/1*ADUvdnyTltZmpubw7w3FPg.jpeg)

3. 并发读写场景下的缓存问题
    数据过期本身还好，比较麻烦的是在分布式环境之下：
    > Readers experienced with distributed systems will notice a couple of such problems that can occur in the caching system described above:
    - In case of high-volume read traffic, a write (and thus a cache value invalidation) can lead to a thundering herd of readers storming the system of record to reload the value into cache.
    - A concurrent read and write can cause a stale value to be stored in cache indefinitely.

4. 引入lease(preventing thundering herds and stale sets)
    这里面使用的方法是facebook的论文中的lease算法，此处相当于给了一个实现。核心是依赖两个操作
    - atomic_add(key, value): set the provided value for key if and only if the key has not already been set.
    - atomic_check_and_set(key, expected_value, new_value) : CAS

5. 引入lease带来的新问题
> Consider a use-case where your data is consistently read frequently, but parts of it also undergo periodic bursts of frequent writes:

> A pathological condition may take place where during stretches of numerous writes, readers end up taking turns acquiring leases and querying the system of record only to have their lease cleared by a write. This in effect serializes reads from the source of truth but doesn’t deduplicate them, which ultimately causes very high read latencies and timeouts as readers wait to get their turn to fetch the value they need from the source of truth.

在高并发读+周期性的写 场景中，会使得读的latency变长，变得近乎线性，一直在等待。解决方法是，成功通过lease拿到数据的reader将数据暂存某个位置，供其它读（同一lease）使用，这个时候虽然数据已经被改了，但是这个数据相对"其它读"发起的时间还是新鲜的。
即[Indeed, if a reader encountered another reader’s lease, the reader must have arrived before the writer cleared the cache value and thus before the write was acknowledged, so both readers can return the value retrieved by the lease holder without sacrificing read-after-write consistency.]
 