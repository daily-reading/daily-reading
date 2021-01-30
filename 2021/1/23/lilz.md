# [ Designing Edge Gateway, Uber's API Lifecycle Management Platform
](https://eng.uber.com/gatewayuberapi/)

- 介绍从 2014 开始至今 Uber 的三代网关架构演进

## 第一代
重点做了 dispatch 服务和 api 服务

- dispatch 服务: 类似于 proxy, 连接 C/S 两端, 根据一定的均衡规则做点对点分发.
- api 服务: 统一 payload 规范(restful, request/response body 使用 json 格式), 提供报文中夹带上下文信息功能.

## 第二代
Uber 早期选型就使用了微服务架构, 直到 2019年, Uber 全站一共有 2200+ 微服务.

- 从应用侧统一 restful 规范架构演进为提取了 API gateway 来做 body 标准化(所有请求都过 gateway 是否有性能问题, 如何扩缩容？)
- 前后端服务解耦
- 提供了多传输协议支持, 通讯上不再强制使用 HTTP/JSON, 而是针对不同业务场景允许更多协议.
- 对于一些公共 api 做了服务化处理(授权, 监控, 审计日志等), 简单来说就是提取公共库
- 支持 streaming payload (常见场景是移动端推送)
- 减少 Roundtrips
    - image metadata 缓存
    - 前端 assets 多 domain 下载 (利用浏览器请求并发特性)
    - 使用了 scatter-gathers layer 层做下游数据桥接 (**没理解**)

第二代网关架构演进过程中遇到的挑战:
#### API gateway runtime 挑战
- 最初 API gateway 组件使用了 nodejs 进行开发.期望利用其高并发支持特性, 随着 Uber 架构组件膨胀, 工程师数量增加.  NodeJs 被工程师的熟悉程度不高, 更多的工程师是写 Java 和 Go 的. 导致对 API gateway 维护变难.
- API gateway 组件自身迭代导致有 2500+ npm 库依赖, API gateway 规格过于巨大.
- 后端微服务通讯协议转型为 gRPC, nodejs 对此原生不支持.
- nodejs 在处理 IOBound 问题并不优秀.

最终导致 API gateway 组件遇到重重问题. 故决定进行改造, 了解到后续转 Go 了(**文章没有细说**)

#### 需要对业务逻辑进行改造
由于需要减少 roundtrips, 需要侵入大量业务代码进行逻辑改造.
在保障业务需求迭代同时需要做重构.
较难实施.

## 第三代
重点在:
- 自洽
- 去中心化
- 提供多 Layer 层 (实际理解下来是将 API gateway 功能分拆)
    - Edge Layer 提供 UI 支持
    - Presentation Layer 提供视图支持
    - Domain Layer 提供多服务命令支持
