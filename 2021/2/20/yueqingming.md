[Engineering dependability and fault tolerance in a distributed system](https://www.ably.io/blog/engineering-dependability-and-fault-tolerance-in-a-distributed-system)

可用性(Avaliability)：where it is acceptable to stop a service and then resume it, such as stopping to change a car tire
可靠性(Reliability)： where continuity of service is essential, with redundant elements continuously in-service, such as with airplane engines

**冗余（redundancy）**：最常用的容错方法

无状态组件：各个请求执行之间相互独立，只要拥有足够的资源，就可以处理失败的情况。

有状态组件：通过服务内部的状态将过去和未来的请求链接起来了，所以服务容错的设计依赖于连续性。

在进行系统设计时，要认为故障时很自然的一件事，是会时常发生的。

容错系统的核心挑战是理解故障的本质，以及如何检测和纠正故障。（既要发现也要修复，发现故障时最基本的）

### 无状态服务的容错设计

设计时需要考虑的问题：

 * How do you survive different kinds of failure?
 * What level of redundancy is possible?
 * What is the resource and performance cost of maintaining those levels of redundancy?
 * What is the operational cost of managing those levels of redundancy?

当要实现多个可用区的冗余时，并不能直接简单的通过负载均衡实现，因为负载均衡可能在某一个可用区。

### 有状态服务设计

错误恢复时：

1. 转移到一个正常的机器上。
2. 保证机器的状态和出错的机器处于同一个时间点上。
3. 需要机制保障上面两个机制有冗余

分布式系统中节点间的共识问题？

节点的健康状况并不是一个非黑即白的问题，可能存在中间状态，难以确定当前节点的健康状态。例如网络问题、长时间 FullGC等导致的异常。


容错机制本身也可能因为资源的问题而失效。


 
