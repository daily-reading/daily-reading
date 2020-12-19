Sytem-to-administrator ratio
一个人可以维护多少系统的比例，是一个丈量维护成本的硬性指标。自动化程度越高的企业，这个比例越高。（amazon好像可以到1w以上？）
这篇paper介绍了一系列设计和开发 operations-friendly services的原则和实践


* Expect failures
* Keep things simple
* Automate everything

Overall Application Design
80%的运维问题源于设计和开发
* Design for failure
    * 硬件设施很多，随时可能失败，如果服务无法自动应对失败，很难扩展
    * 失败预案必须有清晰的路径并且经常被验证。
* Redundancy and fault recovery
    * 设计一个任意节点在任意时间都可能失败的服务，并且不会影响SLA
    * 任意时间主动关停一定的服务，不会影响负载能力
    * Security threat model? <todo>
    * 记录所有可能的组件故障，并且保障任意故障不会影响服务质量
* Commodity hardware slice
    * 组合硬件 性价比 高于 单个大硬件
* Single-version software
    * 低成本的快速开发与演进原则
        * 面向内部部署
        * 老版本不需要支持过久
    * 强升
* Multi-tenancy(多租户)
    * 所有user在一套服务下，不做物理隔离

更具体的实践
* Quick service health check
* Develop in the full environment.
* Zero trust of underlying components
* Do not build the same functionality in multiple components
* One pod or cluster should not affect another pod or cluster
* Allow (rare) emergency human intervention
    * 留出极端情况下的接口，script, test offen
* Keep things simple and robust
    * 很重要，踩过坑
    * 尽可能的拒绝复杂算法或者复杂交互
* Enforce admission control at all levels
    * 边界准入控制，可以以SLA为基准
* Partition the service
    * 分片原则，尽可能以精细的实体为准，可调节，均匀
* Understand the network design
    * 理解网络架构
* Analyze throughput and latency.
    * 了解分析流量和延迟的特点
* Treat operations utilities as part of the service
* Understand access patterns
    * 了解接入的流量情况
* Version everything
* Keep the unit/functional tests from the previous release.
* Avoid single points of failure.

Automatic Management and Provisioning
这篇讲如何去设计自动化的管理。
在我们日常工作，有很多去 异常报警，然后依靠人工介入来恢复。但这样会耗费大量的 7 * 24小时的onCall来做保障，而且在紧急情况下需要做很多tough decision

设计自动回复的系统，需要引入重要的系统约束模型
比如数据库，通过异步同步从库，在非常时期自动主从切换，会造成部分数据丢失。
而如果不切换，则造成系统不可用。
如果在设计之初就用强同步从库，则会牺牲吞吐量和延迟。
所以需要去做权衡。需要去做一些牺牲才能留出自动化的空间。
然而在大规模的服务中，做出一些牺牲能够省略下来的人工成本是几个数量级的。

设计一个自动化的系统需要考量的因素有
* Be restartable and redundant
* Support geo-distribution
* Automatic provisioning and installation
* Configuration and code as a unit
    * 开发的配置和代码，看作一个整体
    * 测试部属和生产部署完全一致
    * 运维部署也看作一个整体进行
    * 想起了12factors里面提到的 Store config in the environment，而在代码整体中的配置，都不需要去做环境特异性配置
    * 测试环境做的变化，在生产环境同步变化，类似于ddl，
* Manage server roles or personalities rather than servers（这个还不太明白）
* Multi-system failures are common
* Recover at the service level(没太明白)
* Never rely on local storage for non-recoverable information
* Keep deployment simple
    * 避免复杂的部署脚本
* Fail services regularly


如何管理依赖？
服务会有很多依赖，而依赖管理往往不被重视。
只有当依赖很重要和复杂，或者单一中心的时候，才变得很清晰。
而在有依赖的情况下，我们要有一些假设来管理预期
* Expect latency.
* Isolate failures
    * Fail fast
* Use shipping and proven components
* Implement inter-service monitoring and alerting
* Dependent services require the same design point.
* Decouple components

这些章节列举了大量的生产经验酝酿出来的指导原则，在构建一个服务 的 时候，需要去考虑什么，注意什么，以什么视角来看到问题，需要照顾到哪些方面。
我们平时工作中或多或少的在做着其中的一些原则，但没有过一个能列举这么全的。需要反复的去思考斟酌，每个准则在什么场景下适用，哪些需要去做权衡。
这篇文章，需要去反复读，做成脑图贴起来

Release Cycle and Testing
Hardware Selection and Standardization
Operations and Capacity Planning
Auditing, Monitoring and Alerting
Graceful Degradation and Admission Control
Customer and Press Communication Plan

 

