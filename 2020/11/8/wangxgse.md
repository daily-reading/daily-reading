# 提交说明
- 编号：NO.15@20210411
- 文章：[Architecting for Reliable Scalability](https://aws.amazon.com/cn/blogs/architecture/architecting-for-reliable-scalability/)

# 读书笔记

当业务增长时，整个架构会经历性能、服务管理运维、安全性等整体复杂性的提升。通过采取可扩展架构，可以避免这些事情
> when a solution scales, many architects experience added complexity to the overall architecture in terms of its manageability, performance, security, etc. By architecting your solution or application to scale reliably, you can avoid the introduction of additional complexity, degraded performance, or reduced security as a result of scaling.

文中提出了几种设计理念
- Modularity
- Horizontal scaling
- Leverage the content delivery network
- Go serverless where possible
- Secure by design
- Design for failure