# All things caching- use cases, benefits, strategies, choosing a caching technology, exploring some popular products
## Caching Prerequisites
- 是否需要cache
- 对数据不一致的忍受程度
- 存什么类型数据
- 是否需要维护事务数据
- 是否需要多node cache
- 采用哪种cache方案
- performance, reliability, scalability, and availability?

## Caching Benefits
- Decreased network costs
- Improved responsiveness 和第一点有重合部分
- Increased performance on the same hardware
- Availability of content during network interruptions

## Caching Data Access Strategies
- Read Through / Lazy Loading
  - 先读cache，（not）再读数据库，（find）更新缓存
  - read基本都是这种操作，要看write的策略
- Write Through
  - While inserting or updating data in the database, upsert the data in the cache as well.
  - 怎么保证同步更新呢，事务操作吗，如果不是原子操作，感觉会有大问题，并发写可能导致脏数据
  - 很多缓存的数据可能永远不会被访问
- Write Behind Caching
  - 更新缓存，缓存异步更新数据库
  - 会有数据丢失的风险 需要有一些回滚的操作，如果数据库更新不成功、
  - 可能导致无序的数据库更新 外键约束
 - Refresh Ahead Caching
  - It’s a technique in which the cached data is refreshed before it gets expired.
- cache aside 没有提
  - 
