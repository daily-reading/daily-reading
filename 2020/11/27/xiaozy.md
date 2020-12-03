# Edgar: Solving Mysteries Faster with Observability
> https://netflixtechblog.com/edgar-solving-mysteries-faster-with-observability-e1a76302c71f

1. 对于构建观测工具的团队来说，问题是如何快速、有效的理解系统的行为
> For teams building observability tools, the question is: how do we make understanding a system’s behavior fast and digestible? 

2. 构建 Edgar 的目的：通过整合 request tracing、log、analysis、metadata，来帮助工程师来分析解决分布式系统中的问题

3. Edgar 与 Zipkin 的区别
    1. Edgar 基于 tracing，并且通过 trace 去关联其他的上下文信息，例如日志、事件、元数据，不仅会指出错误，还会提供细节的上下文信息
    2. Edgar 采集全量 trace，而不是做采样
    3. Edgar 同时面向工程师和非工程师用户

4. Tracing 能够有效的展示一个请求在整个微服务系统中在何时被哪些服务处理，
    1. 能够回答“在哪出错了”的问题，但是不知出错的原因，因此
        - 在 span 上增加 log，携带上服务自身对于错误的描述信息
    2. trace 和服务整体的健康状况和行为是什么关系，只是特殊个例，还是可以发掘出 pattern？ 
        - 从 Telltate 获取这个服务耗时的统计情况，告诉当前 trace 的请求是不是特殊个例
5. Edgar 应该减少工程师排查问题的负担，而不是加重
    1. 提供包含足够信息（trace log analysis）、易分享的问题详情页，方便分享给其他服务 owner 来寻求帮助
    2. 在 UI 上优先展示错误和非正常数据
    3. 允许用户配置服务的 log 信息，例如 log 格式、log store 地址、关键字段等，减少干扰信息

4. 个人感想
    1. 对于一个观测系统来说，发现问题只是第一步，如果能帮助分析定位问题才能真正解决问题
    2. 信息太少和太多体验都不好，需要系统对信息做筛选和排序，减少人花在收集和甄别信息上的精力


> Logs, metrics, and traces are the three pillars of observability. Metrics communicate what’s happening on a macro scale, traces illustrate the ecosystem of an isolated request, and the logs provide a detail-rich snapshot into what happened within a service.
