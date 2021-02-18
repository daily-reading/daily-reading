# Edge Authentication and Token-Agnostic Identity Propagation

* Author: Karen Casella, Travis Nelson, Sunny Singh
* Link: https://netflixtechblog.com/edge-authentication-and-token-agnostic-identity-propagation-514e47e0b602
* 扩展阅读:
 * [Scaling Patterns for Netflix's Edge](https://www.infoq.com/presentations/netflix-edge-scalability-patterns/)
 * [User & Device Identity for Microservices @ Netflix Scale](https://www.infoq.com/presentations/netflix-user-identity/)

在业务规模变大之后，Netflix 将用户身份鉴权的逻辑移动到了它们的接入层网关之中。本篇文章详细分享了边缘身份验证服务 (EAS) 是如何与接入层网关 (Zuul) 一起工作的。
