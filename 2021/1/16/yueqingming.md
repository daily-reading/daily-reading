### 要解决什么问题？
集群的节点需要独占的去访问一些资源。但是节点可能宕机，或者处理过程中暂停。在这些错误情况下，他们不应该保持无限期的保持对这个资源的访问权限。

### 解决方案
Wall Clocks are not monotonic
> 计算机有两种不同的机制去展示时间。wall clock time 和 monotonic time.wall clock time 因为进行 NTP 进行对时，所以会有可能时钟回拨的情况。而 monotonic time 没有类似的情况。wall clock time 对应 Java 中的 API 是 System.currentMillis.monotonic clock time 对应 Java API 是 System.nanoTime.

![3ae6a61080f4828a4589973cb55521ec.png](evernotecid://7F01B206-A616-4EA4-AABF-0C5FA9B75355/appyinxiangcom/26825047/ENResource/p178)
![d387cf3f92dc173b4ba34b235e4852cd.png](evernotecid://7F01B206-A616-4EA4-AABF-0C5FA9B75355/appyinxiangcom/26825047/ENResource/p179)
![623b6c0d9a9a0e399abc5b5d9ff5a7fe.png](evernotecid://7F01B206-A616-4EA4-AABF-0C5FA9B75355/appyinxiangcom/26825047/ENResource/p180)
![2871376356ae2b33dc8a6936f4fe5c3d.png](evernotecid://7F01B206-A616-4EA4-AABF-0C5FA9B75355/appyinxiangcom/26825047/ENResource/p181)
![d988edd186cbdab02324180322a8384e.png](evernotecid://7F01B206-A616-4EA4-AABF-0C5FA9B75355/appyinxiangcom/26825047/ENResource/p182)

1. 只有 Master 能够进行 Lease 超时时间的维护。
2. 当 Master 宕机之后，由新选举的 Master 重置 Lease 的超时时间，然后重新开始维护。

Lease 和分布式锁的区别？
多了一个超时时间的维护？

这里 Master 宕机之后新的 Master 的选举过程？
