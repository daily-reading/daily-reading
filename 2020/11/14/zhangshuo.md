原文：https://engineeringblog.yelp.com/2020/11/minimizing-read-write-mysql-downtime.html

### 现状
* 高 QPS
* 应用程序通过七层代理连接 SQL 服务器
* 一个主节点，主从异步复制
* 服务发现使用 zk
* Orchestrator，用于服务器高可用性和故障检测

当 MySQL 要切主时，如何最小化停机时间
1. MySQL client 仍保持与代理的连接
2. Orchestrator 几秒内检测到故障，选主并注册到 zk
3. 代理层监视到数据更新，同步配置

### ProxySQL as a highly available proxy layer
支持连接数限制，替换数据库服务器时不需要建立新的连接

#### 配置 ProxySQL 路由到 MySQL 后端
1. ProxySQL 主机组功能将 MySQL 分为 schema 和 role
2. schema 隔离工作负载，role 隔离读写、只读和非用户读流量
3. 每个用户唯一映射到一个主机组
4. ProxySQL 服务器配置一组可用 MySQL 服务器，每隔几分钟，ProxySQL 上脚本定时从 zk 读数据，刷列表

#### 应用程序连接到 ProxySQL
1. ProxySQL 也注册到 zk
2. 应用程序提供用户名和密码给 ProxySQL，以初始化 MySQL 连接

### Service Discovery
通过每个服务器的守护进程维护服务发现的数据。守护进程认为服务健康，则在 zk 记录服务信息，认为不健康，则删除失败的服务实例信息

#### MySQL 健康检查
1. 每个 MySQL 服务器上部署一个 HTTP 服务，作为健康检查端点
2. 执行监视框架定义的所有监视检查，如端口是否打开等

#### ProxySQL 健康检查
1. 只需要侦听定义的 ProxySQL 端口的 TCP 连接即可

### Orchestrator driven Failure Recovery
如果副本的复制源发生故障，则重新配置正确的副本。如果是主发生故障，继续将主设为只读，同时选取候选人提升为主，重新配置信息

#### 主的故障迁移 hook
1. hook 负责向 primary 发送 http 请求，来更新角色信息
2. 守护进程注意到 primary 的角色变化，向 zk 更新数据

### Perspective of a MySQL client during a primary failover
1. 在 ProxySQL 更新主机组配置前，客户端无法插入删除操作
2. 更新完成后，不需要新建连接，ProxySQL 服务器能将 MySQL 路由到新服务器

### Special case: Split-brain
1. 网络分区可能引发脑裂
2. 这里将服务发现数据改为 ProxySQL 的主机组配置，并校验，如果 zk 中注册了一个以上的主机组，则拒绝更改主机组配置
3. 修改 Orchestrator 配置，保证不会在单独的数据中心中选择新的 MySQL 主节点


