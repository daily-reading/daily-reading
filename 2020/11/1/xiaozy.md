# Strategies Used at Box to Protect MySQL at Scale
> 原文地址 https://medium.com/box-tech-blog/strategies-used-at-box-to-protect-mysql-at-scale-35388b85f2a4

1. DAI(Data Access Infrastructure) 的约束
    - 强数据一致性：不能读到部分更新的数据、写后读场景一定要能读到更新
    - 高吞吐
    - 低延迟
    - 高可用
2. Data Access Service: 单体 -> 读写分离 & 水平分片（业务服务抽象数据访问层） -> 数据访问服务
3. 改善副本使用：主库 load 增长太快，而从库又无法接读流量（一致性约束） -> 从库一致性读
    - 检查从库是否追上主库
    - 如果从库没追上主库，则有对策
        1. 挂掉请求（只能应对追不上的情况比较少的场景）
        2. fallback 回主库（持续 lag 时，读写都打到主，会把主打崩）
        3. 等待从库追上主库并一直重试（去打保证从库追上的时间，从而会导致超时）
    - 最终方案：client 只用等到一个副本同步到了它的写操作，而不是所有的写操作
        - 关键的流量特点
            - 副本追求写后读的一致性，而牺牲一些 latency
            - 大部分读操作都在更新后数分钟、数小时后，对一致性收敛速度要求不高
    - 个人感谢
        - 优化一定是和业务场景相关的，需要根据业务场景做针对的优化
4. 改善缓存使用
    - 问题：读副本导致 cache 容量变小，从而 cache miss 升高
    - 原因：
        1. 使用的 look aside cache，读时 load 缓存，写时缓存失效 -> 读主还是读从直接影响 cache -> 只读主
    - 解决策略
        1. 从追上主的从那里缓存值
        2. 缓存"一个对象不存在"这个事实，因为很多请求是看一个对象是否存在，缓存这个知识能够减少访问数据的次数
        3. [lease](http://www.cs.utah.edu/~stutsman/cs6963/public/papers/memcached.pdf)
    - 流量模式
        1. 读操作要求写后读一致性
        2. 查询记录是否存在是个高频请求
        3. 短时间内记录的读写压力很大
5. 限流
    - 问题：根据请求访问的 db 的资源状况进行限流，会导致流量不均衡，产生尖峰、容易产生单点问题
    - 需求约束：
        - Decentralized infromation propagation: 不用共享存储来保存所有信息，而是在在服务里保存
        - Fault tolerant: 限流必须得能处理 host 加入或离开集群
        - Fast convergence： 快速收敛
6. QoS for Writes
    - 问题：异步写越来越多 -> 严重的 repliace lag -> 高可用风险、奖励了读策略的有效性
    - 目标：给一步价动态的施加背压（back pressure），从而降低异步写的速度
    - 方案：
        - 在客户端给写操作设置 QoS，high QoS for customer path
        - high QoS 随时可写
        - low QoS 只能在 replica 没有严重 lag 的时候可写
        - medium QoS for custom triggered async triffic，保证 medium QoS 流量被拒绝时 low QoS 流量无法执行
            - customer write 可以延迟并重试
            - 内部写可以丢弃
