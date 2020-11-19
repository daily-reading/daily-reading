Key goal of Caching: lowering latency and load in read-heavy enviorment.
Cache makes problem
* 一致性问题
* 调试难
    * Cache 转瞬即逝
    * 神秘的一层
如何能够有好的响应和负载，就尽量不要用缓存
两种常见的缓存策略
* write-through
    * 写记录同时更新缓存
* Populated upon read
    * 读记录失败，降级读库，写缓存
本文注重于 populated upon read

分布式场景下可能出现的一些问题
* 高读流量的场景下，一个写操作可能会导致惊群效应，让一大堆读操作尝试去写缓存
* 并发的读写，可能会导致缓存数据过时，不一致

解决上述问题的方法，由 <Scaling Memcache at Facebook>
Lease概念
由两个基本操作组成
* atomic_add (key, value) 
    * memcached.add
    * redis.setnx
* atomic_check_and_set( key, expected_value, new_value)
    * memcache.cas
    * Redis through Lua Script to do so

* 尝试去获取cache value
    * 获取失败
        * 尝试 setnx 设置一个lease (这里应该有一个超时时间？)
            * 设置成功
                * 获取真实值，从数据库
                * CAS 追加值 atomic_check_and_set(key, lease_value, true_value)
            * 设置失败
                * 尝试再次获取，回到第一步
    * 获取成功
        * 判断获取到的值是否是 lease
            * 是
                * sleep for a while
            * 否
                * 返回值，结束流程


下面是一个更复杂的场景：持续高读+周期性的写入尖峰
周期性的写入尖峰，会使得大量缓存失效，影响读场景，使得读场景的lease获取与等待时间拉的很长。

解决方案就是
获取lease失败的readerA，可以返回已获取lease成功的readerB取回的“真实值”，虽然这个真实值本身已经可能被writer的写入行为给过期掉了，使得readerB的atomic_check_and_set操作失败
为了实现这个目的，readerB会将取回的数据放置于某处供其他reader取用。
因为这个场景下，readerA 获取 lease失败，一定是发生在 writer 生命缓存失效之前，所以这里不会出现写后读的一致性丢失问题。

总体感觉
后面给了真实的代码
看了几遍后，感觉，自己好弱。

