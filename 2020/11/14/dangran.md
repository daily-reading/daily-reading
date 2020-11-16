
讲述在Yelp, 他们是如何尽可能降低MySQL的宕机时间以及自动恢复

Yelp MySQL基础设施包括
* 极高的QPS
* ProxySQL 组成的七层代理
* MySQL集群，一主多从，异步从。跨机房部署
* 基于ZK的服务恢复系统
* 开源的Orchestrator，在不同数据中心间通过Raft协议来做高可用和异常检测

MySQL 异常当即 & MySQL主动升级导致主从切换
如何做到尽可能小的不可用时间？
* clients保持对proxy的连接
* Orchestrator 秒级检测失败，立即进行主节点切换
* 新主主动告知 服务恢复系统完成主节点切换
* proxy监视着服务恢复系统，并自动加到自己的配置里

ProxySQL 高可用proxy层
在 几个方面
* 部署
* 路由到MySQL server
    * 路由
    * 扩容
    * 切换
    * 负载均衡
    * 读写分离
    * 用户管理
    * Zk服务注册&发现 

Orchestrator 来做哨兵与恢复 : https://github.com/openark/orchestrator

总体感受

Mysql proxy层似乎实现了无限的可能

MySQL proxy层的设计和 MySQL client的设计 各自有什么优劣呢？

本文中proxy提到的 路由、扩容、切换、负载均衡、读写分离、用户管理等，似乎通过client也能完成
client的设计等于将所有能力分散在所有的客户端，各自去做，proxy就是将能力集中起来


