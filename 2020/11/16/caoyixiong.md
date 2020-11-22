# Building Netflix’s Distributed Tracing Infrastructure

## 本文主旨
本文主要讲了Netflix的分布式跟踪系统的建设

## 主要内容
1. 没有tracing的时候重建会话是非常复杂且麻烦的
2. 选择使用ZipKin，因为其与SpringBoot的更好的集成性
3. 使用Mantis进行trace数据的流处理，Cassandra进行存储Trace
4. 根据请求path设置不同的采样率
5. 存储优化
   - 最开始使用ES存储，后面因为写入量太大转变为Cassandra
   - 选用了更便宜的EBS替换SSD
   - 压缩存储数据
   - 只存储感兴趣的trace数据，比如有warn，error，retry标签的
   - 使用机器学习技术
   - 冷热数据隔离，热数据存储Cassandra，冷数据存储S3
6. 其他的优势
   - 应用程序监控检测
   - 弹性工程    
   - 区域疏散
   - 估算运行A/B Test的成本
7. 未来计划
   - 推广Trace
   - 增强查询跟踪数据的分析能力，并且能够自定义面板
   - 整合Metric，Logging和Tracing

## 总结
国内用Cassandra还是太少了，大家貌似都是用的ES
后面研究下Cassandra，感觉可以为SkyWalking提供一个新的存储方式