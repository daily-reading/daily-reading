# Edgar: Solving Mysteries Faster with Observability
- metric等观测工具只能展示系统的健康状态，需要更细粒度的关泽，need to know why a specific request is failing and where
- Edgar
  - request tracing and additional data from logs, events, metadata, and analysis  因为trace所提供的有价值的数据是有限的，需要整合日志，时间和元数据
  - captures 100% of interesting traces  如何定义intersting，流量分优先级？ 现在我司只对服务区分了优先级
  - a powerful and consumable user experience to both engineers and non-engineers alike  skywalking前端的用户体验不太行
## Tracing as a foundation
  > Metrics communicate what’s happening on a macro scale, traces illustrate the ecosystem of an isolated request, and the logs provide a detail-rich snapshot into what happened within a service. 
- span
  - Represents a unit of work
  - relationship with other spans
  - Contains a set of key value pairs called tags
  - Has a start time and an end time
## Adding context to traces
- 只有trace可以发现错误的发生地点，但是不知道发生了什么   需要添加log  前提是log带有足量的信息
- trace是异常行为还是普遍行为。 skywalking带有一些对trace的分析，应该可以handle部分场景，但是前端不够易用


  
  
  
