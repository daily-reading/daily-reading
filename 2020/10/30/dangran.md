

Part1 A Structure for Scaling Service Development

JSON-over-HTTP   —> Thrift-Over-HTTP
Emitting standard request and response metrics on both server-side and client-side not only allows for better monitoring in and outside the service-owning team, it can be leveraged for creating automated service dashboards and alerts. 

集成式的Service IDL 标准
* 支持Java，Ruby
* Generated RPC client support standard metric,dashboard,TLS,annotation to tag a idempotent method
快速高效的支持了微服务的扩张。


Part 2
如何做到标准化？
如果没有标准/最佳实践，会产生哪些问题？
* POJO 标准不一致
* JSON 对象 配置不同
* 请求上下文不一致
* 不标准的 metrics
* 报警缺失
* 技术栈不一致
* 不同的RPC timeout, circuit-breaker, retry 逻辑
总结一下，inconsistency

经典语句:
The most effective way to enforce adoption of standards and practices is to build them into the service platform on which the engineers develop their services.  

我们做了什么？
通过IDL 来标准化生成 service, client 代码
集成标准的 context
* userId/visitorId
* Ip,locale,currency
* Auth policy check logic 
* Rate-limiting based on userId logic
* Standard metric, dashboard
    * request.count, tags
    * response.count, tags
    * Median P99
    * Exception info etc.
* alert
    * Method-level, service-level annotation to configure alert
    * Tool to generate full set of standard alert from IDL
    * Default configure


Part 3 - Resilience is a Requirement, Not a Feature
Resilience 是硬需求！
* Async Server Request Processing
    * 提升吞吐量
    * Starvation prevention
        * 避免高开销的请求影响低开销的请求
    * Request Queuing
    * Load Shedding
        * aggressive client retries 影响很大，不会给下层恢复的机会


Part 4
* 为什么说面向 schema 的基础设施是对测试不友好的？
API is the boundary between services
    * Breaking Change API
    * 可用Mock数据的缺失
    * 数据的逻辑正确性难以保证
    * API 鉴权 缺乏统一的支持
    * Real time Metric 缺失
Focusing on the schema aspect of service API testing

总结一下：
四篇介绍了 Airbnb是如何一步一步构建 基础设施对 高效SOA的 能力
从始到终，一步一步的将整个思路，遇到的难点，如何解决，构建了哪些能力，慢慢的解构，展现。
展现了一个较为成熟的standard infra 基础架构的图景 
cc 大佬们
