# How We Learned to Stop Worrying and Read from Replicas
两种策略解决 read-after-write consistency
- wait for writes to make their way to replicas before acknowledging them  同步
- delay user reads until replicas are caught up to the master
- 一个有概率地降低读效率，一个降低写效率

## Common Approaches for Dealing with Asynchronous Replication
 replication-lag-sensitive, but latency-tolerant read
 1. Get the replication position from the master 
 2. Poll positions of replica databases until one passes the position 
 3. 从库已经catch up，直接读从库
 
 - higher latency
 - 如果timeout再去读主库是很危险的，lag往往可能就是主库压力过大
 
 因此上述的方法会因为以下条件受限
 - High latency in times of reasonable replication lag
 - Persistent failures in times of severe replication lag.
 
 ## An Alternate Approach
- 不判断是否同步，判断与读操作相关的写是否已经同步，即使整个 database replication 会有 lag 也没关系。这个 position 和资源的关系需要映射到 data access layer 做记录
