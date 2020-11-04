# Cache is the Root of All Evil
caching在有效减少lantency的同时引进了复杂的正确性问题
>  It is almost a law of nature that once you introduce a denormalization, it’s a matter of time before it diverges from the source of truth.
- 不规范但是有效的手段带来的问题可能更多
- 能不加cache就不加，比喻加caching是在和恶魔定契约

## 正文
- cache invalidation is the hard part. 
  - 一次写操作使record可能会导致惊群效应
  - 并发读写可能导致缓存了失效的值
- leases（分布式系统中很多地方都用到这个概念）  
- 但是在某些情况下可能会导致read操作延迟变大（在文章图中所描述情况，高并发写时，其实cache不可避免就失效了）

## 总结
- 感觉今天的这篇caching的文章，它的方法有点复杂啊。
- 使用caching本来就是为了可用性而牺牲掉一些一致性的。预期应该就是会数据不太一致的。强一致的需求就走数据库，弱一致就走缓存，这种成本是不是更低一点
