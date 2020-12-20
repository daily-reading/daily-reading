### 三个原则
* Expect failures
    * 组件可能会崩溃或随时停止。
    * 网络故障
    * 磁盘空间不足

* Keep things simple
    * 复杂性会滋生问题
    * 简单更容易保持正确

* Automate evarything
    * 人总是会犯错，需要工具来保证流程上的正确。

开发、测试、运营不是割裂的，而是紧密相关的。




* * *

### Overall Application Design
* design for failure
    * The entire service must be capable of
surviving failure without human administrative
interaction
    *  if the failure
paths aren’t frequently used, they won’t work
when needed(有点类似与 GitHub 故障的那篇，各个措施都失效了）

* Redundancy and fault recovery.
    * 找到系统潜在的问题，并且为每种问题都实现应对措施。
    * 写下所有的故障组合，保障服务在每种故障组合下都能正常运行。
    * 很小概率出现的故障可以不考虑处理方案，投入和产出不成比。

* Commodity hardware slice. 
    * 大型集群比少量大型服务器更便宜
    * 服务性能更高
    * 功耗
    * 故障转移时，分片的某台服务影响更小。

* Single-version software
    * 面向内部部署
    * 旧版本不需要支持太久
* Multi-tenancy
    * 多用户在同一套环境下，不需要隔离。

* Quick Service health check
    * this is the services version of a build verification test.
    * not all edge cases are tested, but if quick health check passes, the code can be checked in.

* Develop in the full environment.
    * 不仅仅进行单元测试，还要进行整体的进行集成测试

* Zero trust of underlying components.
    * 继续以只读模式在缓存数据上进行操作
    * 访问冗余副本


* Do not build the same functionality in multiple
components
    * 难以预见未来代码的改动，冗余将导致修改扩散。
    * 如果不注意，将导致代码库迅速恶化。
    
* One pod or cluster should not affect another pod
or cluster

* Allow (rare) emergency human intervention.
    * 允许人为干预系统，但是将人为干预的步骤工具化。

* Keep things simple and robust.
* Enforce admission control at all levels
    * 不然让更多的流量进入过载的系统，导致系统崩溃。
    * 各个级别都应该有限流控制。

* Partition the service
    * Partitions should be infinitely-adjustable and fine-grained, and not be bounded by any real world entity (person, collection ...).

* Understand the network design
* Analyze throughput and latency
    * 指定用于容量规划的度量标准
        * 每个系统每秒用户请求
        * 每个系统的并发在线用户

* Treat operations utilities as part of the service
    * utilities 也应该经过代码审查和测试
* Understand access patterns
    * 设计任何功能时都需要考虑其对存储层的负载

* Version everything.
    * 在生产测试期间要保证版本 n 和 n + 1是兼容的

* Keep the unit/functional tests from the previous
release.
    * 保证版本 n - 1的功能是正常的
* Avoid single points of failure
    * 无状态服务
    * 服务冗余
*** 
### Automatic Management and Provisioning
* 报警然后人工介入有时候并不是一个好的模型，最好的是有自动恢复的策略。
* 自动化的管理和配置需要在设计阶段就开始计划

*** 
Best practices:

* Be restartable and redundant（可重启和冗余）
* Support geo-distribution. （异地多活？）
* Automatic provisioning and installation（自动配置和安装）
* • Configuration and code as a unit. 
    * 配置和代码作为一个单元提供
    * 作为一个单元部署

* Manage server roles or personalities rather than
servers
* Multi-system failures are common
* Recover at the service level
* Never rely on local storage for nonrecoverable information.
* Keep deployment simple（docker 的兴起）
* Fail services regularly
    * 验证补救措施是否有效（混沌工程）
*** 
### Dependency Management
best parctices:

* Expect latency
    * 设置超时，避免长时间占用资源
    * 操作幂等性，超时后可重试
* Isolate failures
    * 快速失败，熔断降级。

* Use shipping and proven components
    * 生产环境所需要的是稳定，对于升级版本来说，少许性能的提升所带来的的不确定性是得不偿失的。

* Implement inter-service monitoring and alerting.
    * 服务出现问题时，通知到相关工程师

* Dependent services require the same design point.
    * 所依赖的服务至少需要提供与你的服务等同的 SLA 保证

* Decouple components

*** 
### Release Cycle and Testing

* 生产环境有足够的冗余性，发生新服务故障时能够快速的恢复状态。
* 数据损坏与状态相关的故障极不可能
* 必须检测错误，并且必须监视测试中代码运行的情况
* 必须可以快速回滚所有更改，并且必须先测试此回滚方案。

*** 
best practices：
* Ship often
    * 发布周期不要太长
* Use production data to find problems
    * Measurable release criteria(可测量的发布标准)
    * Tune goals in real time （实时调整目标）
    * Always collect the actual numbers （收集指标而不是报告）
    * Minimize false positives.（尽量减少误报，减少噪音）
    * Analyze trends。（分析趋势）
    * Make the system health highly visible(使系统运行状态高度可见)
    * Monitor continuously.（持续监控，每天查看所有数据，是团队工作的一部分）
* Invest in engineering.
    * Good engineering minimizes operational requirements and solves problems before they actually become operational issues.
    
* Support version roll-back
    * 不能回滚的发布是有很高的风险的。

* Maintain forward and backward compatibility（保持向前向后兼容性）
    * 在确认不会回滚回旧格式之前，不要淘汰对旧格式的支持。

* Single-server deployment.
* Stress test for load.（至少保留两倍的负载冗余）
* Perform capacity and performance testing prior
to new releases
* Build and deploy shallowly and iteratively
    * 用例级别的思考？
* Test with real data
    * 收集真实环境的数据，用于测试，并且保证不会对真实生产环境产生影响。
* Run system-level acceptance tests
* Test and develop in full environments

*** 
### Hardware Selection and Standardization

硬件标准化需要遵守的两个原则：
* 所有服务都是小型服务，都在使用自动管理和配置基础架构
* 新的服务可以更快的被测试和部署
*** 
best practices：
* Use only standard SKUs
* Purchase full racks
* Write to a hardware abstraction
* Abstract the network and naming.

*** 
### Operations and Capacity Planning

* 不要使用没有经过生产验证的东西


* Make the development team responsible
* Soft delete only
* Track resource allocation
* Make one change at a time（减少变量）
* Make everything configurable. （配置修改更容易、更安全和更快）

*** 


### Auditing, Monitoring and Alerting

> 所有的更改都需要保留记录，否则出现问题时不知道是那些更改导致的。

报警是否有效的两个指标：
* 警报与故障单的比例（目标接近 1）
* 没有相应警报的系统运行状况的问题数

---
best practices：
* Instrument everything
* Data is the most valuable asset. 
* Have a customer view of service.（测试是要有意义的）
* Instrument for production testing.(完整的监视和警报）
* Latencies are the toughest problem (延迟是最棘手的问题)
* Have sufficient（足够的） production data.
    * Use performance counters for all operations.
    * Audit（审核） all operations.（所有改变状态的操作都需要记录，回溯的过程）
    * Track all fault tolerance mechanisms（容错机制）
    * Track operations against（针对） important entities.
    * Asserts
    * Keep historical data
* Configurable logging.（可以不重启进行日志打印的控制）
* Expose health information for monitoring
* Make all reported errors actionable（错误报告中需要指明具体的错误信息，和可能的修复措施）
* Enable quick diagnosis（诊断） of production problems.
    * Give enough information to diagnose. 
    * Chain of evidence（证据）
    * Debugging in production
    * Record all significant actions
    
    
***
### Graceful Degradation and Admission Control

* Support a ‘‘big red switch.’’
    * 紧急情况下释放非关键负载（对某些非关键链路进行降级）

* Control admission
    * 排队机制，控制进入机器的负载

* Meter admission（服务的缓慢恢复，背压？）

*** 
### Customer and Press Communication Plan

* 出现故障时与客户沟通的渠道
* 出现某种故障时与客户交流的话术



设计一个互联网服务时应该考虑那些点，文章里面一应俱全不。过很多点都是简单的一笔带过，想要真正的在实践中应用起来，还需要具体的去学习相关的实现，了解对应的实际的解决方案。这篇文章更多的像一个清单。






    
