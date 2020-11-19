> 原文地址
> https://medium.com/airbnb-engineering/building-services-at-airbnb-part-1-c4c1d8fa811b
> https://medium.com/airbnb-engineering/building-services-at-airbnb-part-2-142be1c5d506
> https://medium.com/airbnb-engineering/building-services-at-airbnb-part-3-ac6d4972fc2d
> https://medium.com/airbnb-engineering/building-services-at-airbnb-part-4-23c95e428064

## Part 1
1. Airbnd 面临的问题：无法扩展开发活动(scaling development)
    1. 缺乏定义清晰的、强类型的服务接口和数据模式
        - 在不看源码的情况下，不知道这个服务是做什么的、如何调用
        - 缺乏 contract validation，导致不兼容修改引起生产事故
    2. HTTP 服务框架缺乏完善的 RPC 生态，例如标准基础设施实现、最佳实践、监控告警等
    3. JSON 作为数据负载，体积太大效率太低
2. 备选方案
    1. 直接迁移到成熟的方案，例如 gRPC、Thrift
    2. 扩展 Dropwizard，Thrift-over-http
        1. 使用 Thrift 作为 IDL
        2. 使用 Thrift 作为 HTTP Payload
        3. 强类型 API
            > if service developers were restricted to too small a set of types they would end up reshaping the types they want through existing types (e.g, date as a string), bypassing the automated type-checking and validation, and defeat the purpose of having a strong-typed schema.
        4. 扩展 ApacheThrift，同时生成 Thrift 和 Dropwizard 代码，同时支持 Thrift 和 JSON，提供无缝体验
        5. 形成统一的最佳实践
            1. end-to-end request context support
            2. Ready-to-use tls support
            3. standardize client and server side metrics
            4. 幂等接口支持注解来指定重试

---

## Part 2
1. 为什么需要一个标准的服务平台
    - 缺乏标准时，不同的服务具有各自的实践，会造成不一致
    - 文档化的标准和建议性质的最佳实践往往不会有用，业务工程师总会忘记或者晚点再搞，因此最有效的方式是平台把最佳实践提供出来
      > However, merely documenting standards and recommending best practices for engineers to follow do not work. Engineers often forget them or push them off until later (and then forget) when focused on coding business logic to meet product deadlines. The most effective way to enforce adoption of standards and practices is to build them into the service platform on which the engineers develop their services.
2. 标椎内容
    1. Request and Response Context
        - 最少包括 userId，还可以包括 IP locale Country
        - 场景
            1. 用户/地区级别的特性灰度、策略实验
            2. Auhtorization
            3. RateLimiting
    2. Standard Metrics and Dashboards：server/client side request/respose count error
    3. Service API Alerts
        - 通过给服务或 API 加注解来设置报警规则

---

## Part 3
1. Resilience is a Requirement, Not a Feature
2. 共性的稳定性问题
    1. request spikes
    2. system overload
    3. server resource exhaustion
    4. agressive retry
    5. cascading failures
3. works
    1. Async RPC
        - 提高了并发，很少的 IO 线程就能提供很高的并发
        - 处理流量尖峰时不会导致线程池被打死
        - 避免饥饿
    2. Request Queuing: Controlled Delay queue witha adaptive LIFO
        - Controlled Delay queue
            - 正常情况下，Server 都能在 N 毫秒内处理完请求，并清空队列，将 N 作为正常的 timeout
            - 如果 N 毫秒内队列不为空，则用一个更大的 timeout，以此防止因为客户端重试打挂 server
        - Adaptive LIFO
            - 请求正常处理时是 FIFO
            - 请求开始排队候是 LIFO，因为后入队的请求更不容易超时
    3. Load Shedding
        1. Circuit Breaker 保护过度重试
        2. Service Back Pressure: Server 压力过大是 fail fast，同时 client 也 fail fast，将整个调用链路的压力降下来，使得 server 能够有时间恢复
        3. API Request Deadline：Deadline 随着整个调用链路走，Server 检查 Deadline 如果到了 Deadline 直接 Fail fast
        4. Client Quota-based Rate Limit： Server 为不同的 Client 设置 Quota
        5. Dependency Isolation and Graceful Degradation: Bulkhead 隔离依赖
        6. Outlier Server Host Detection：客户端在做负载均衡时，考虑 Server 实例的健康状态、性能指标

---

## Part 4
1. 为什么面向 schema 的基础设施对服务测试至关重要
    1. Breaking API changes：API 破坏性变更容易导致故障
    2. 服务使用放缺乏可用的 Mock 数据：工程师 mock 数据的工作量很大
    3. 缺乏 Mock 数据语义正确性的保证：手写的 mock 数据很容易碎业务演进而过时
    4. 确认服务 Owner 的验证机制：服务 owner 无法快速简单的验证 API
    5. 对于 API 测试质量，缺乏事实的度量
    6. 测试不够轻量和可扩展
2. schema-oriented infrastructure benefits
    - service owners
        1. Static API Schema Validateion：自动的兼容性 lint & best practice 检测
        2. API Integration Testing Framework：不用自己写 API 验证代码 & 实时 metrics
            - 不用自己写 API 验证代码：用预定义的数据，在生产环境实例进行测试
    - service consumers
        1. API Mocking Framework: 不用自己写 mock
        2. API Integration Testing Framework： 保证语义和提供方一致
