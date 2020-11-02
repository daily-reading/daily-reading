# Strategies Used at Box to Protect MySQL at Scale
  data access infrastructure guarantees: 
 - strict data consistency
 - high throughput
 - low latency 
 - high availability
 很难同时保证四个guarantees
 
 业务扩大->水平拓展
 ## Adding a Data Access Service
 - 加入这个service是为了解决数据库连接 exhaustion
 ## Improving Utilization of Replicas
- 异步复制导致副本数据一致性不高，不可用。主库压力很大
- 使用一种算法检测replica是否catch up
  - fail the request 可能导致一直失败
  - fall back to the primary 可能会把主数据库打挂
- 这两个方案都在replica is already caught up时效果比较好，这是无法保证的
- the only other option is to wait for the replica to catch up and keep retrying until it is.
  - 无法保证replica will catch up to the primary in a reasonable amount of time
- 保证read-after-write consistency就可以了

## Improving Cache Utilization
- 缓存完全catch up 主库的从库读数据
  - fully catch up 的情况发生的概率很大
-  cache the fact that an object doesn’t exist. 
  - 10% of our cache hits today are to non-existent values
-  cache lease
  - 部分值是热点值，高频读写
  - 解决惊群效应
  
## Rate Limiting at the Data Access Layer
- 单点资源
  - 增加latency
- requirement
  - Decentralized information propagation 
  - Fault tolerant
  - Fast convergence
-  Push-Sum Protocol

## Quality of Service for Writes
异步写越来越多
- resulted in significant replication lag
- putting our high availability at risk and rendering our replica read strategies less effective
做法
- 分优先级 提供feedback loop to client，使得client可以控制自己的速率 削峰
