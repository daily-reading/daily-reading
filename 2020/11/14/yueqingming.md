#### Yelp Mysql Mysql 特点：
* 成千上万的 HTTP 用户请求以及批处理请求
* 通过 ProxySQL 连接 Mysql Server。（七层代理）
* 单主节点、异步复制、跨机房部署主从
* 基于 ZK 做 Mysql 的服务注册发现
* 通过 Orchestrator 实现高可用复制管理
#### Mysql 主从切换过程
1. Mysql客户顿保持和 Proxy 的连接
2. Orchestrator 在秒级检测到异常，然后开始恢复措施，选取出一个新的主节点。
3. 新的主节点向 ZK 注册当前节点为集群的主节点。
4. Proxy 层发现主节点变更，切换配置中的主节点。
![128d7604c3290c6ebdf5c9f1e192606b.png](evernotecid://7F01B206-A616-4EA4-AABF-0C5FA9B75355/appyinxiangcom/26825047/ENResource/p74)
实现：
* 所有组件向 ZK 注册当前身份以及唯一标示（IP），并且消费需要的数据。
* 应用程序建立与 ProxySQL 的连接，并发出查询。
* ProxySQL 保持与所有 MySQL 节点的连接，并把客户端的连接代理到这个连接池。
* Orchestrator 保持与所有 Mysql 服务端的连接，不断发送心跳检测，并在需要的时候执行故障恢复策略。

Mysql 角色区分：
primary, replica, reporting replica 有点类似与现在我们的使用现状 主节点 读节点 pipe节点

每个 Mysql 服务上运行一个守护线程，读取机器上的文件配置，并与 ZK 维护响应的配置。

怎么判断一个 Mysql 节点是否健康？

* 服务器从备份还原
* 服务正在复制并实时更新
* 通过从群集中的另一台服务器流式传输MySQL缓冲池并将其加载到其自己的缓冲池中，从而对服务器进行“预热”

全自动的读写分离

Orchestrator 中也需要 Mysql 服务的相关信息，也是在启动的时候去 ZK 中获取吗？

ProxySQL 和 Application 融合在一起？


