# A Distributed Algorithm for Deadlock Detection and Resolution

* Author: Don P. Mitchell, Michael J. Merritt
* Link: https://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.88.2416&rep=rep1&type=pdf

## 摘要
论文描述了一种通过构造有向图(wait-for graph)的算法来检测分布式场景中的死锁问题，其本质是图的回路可以通过沿图的边传播一种
探测器来检测。当一个发起者得到一个与自己发送的探测器相匹配的探测器时，则说明形成死锁。具体到论文中的实现是在 wait-for graph 中的每个节点都拥有一个 private 和 public label 来描述节点状态, 一开始这两个 label 是相同的。状态步骤具体分为4种:

1. **Block step**: 当一个进程在等待其他节点释放资源时，系统会在这两个点之间构造一条有向边，指向锁资源持有节点。此时等待的节点会
将自己的两个 label 替换为一个大于对方节点 public value 的值。
2. **Activate step**: 如果 timeout 或者锁被释放，或者锁资源被转移到其他节点了，这条边就会消失。
3. **Transmit step**: 当进程读取拥有锁资源的进程的公共标签时上，如果发现对方的 public label 比自己的大, 就会用它替换自己的 public label
。这样做的结果是，wait-for graph 中较大的 public label 会随着该状态的出现进行转移。
4. **Detect step**: 当进程发现它读到的 public label 和自己的是一样的，意味着出现了一个死锁。这个时候就需要终止进程来释放死锁了。

### 最小优先级中断

因为系统中里存活时间最久的节点更容易出现死锁，所以优化的方法是在上述算法之上增加了优先级，在 Transmit 阶段会将自己的优先级变更为自己和对方中较小的那个。在系统检测到出现死锁后会优先干掉优先级最小的节点。


## 结论

一般来说，微服务 SOA 架构中的服务的依赖可见性做得好的话，在服务立项评审时就可以避免出现依赖循环的情况。当整个系统的依赖关系还处于比较黑盒的时候或者有什么无法避免的原因一定会有循环依赖产生时，则该死锁检测方法就可能可以发挥作用了。
