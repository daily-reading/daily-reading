什么是分布式优先级队列？
![7fa0608cf36a95eb806cd9f0e49b188a.png](evernotecid://7F01B206-A616-4EA4-AABF-0C5FA9B75355/appyinxiangcom/26825047/ENResource/p412)
为什么需要分布式优先级队列？

* 异步化
* 流量削峰

在 FaceBook 的具体使用场景？

* Facebook asynchronous compute platform
* video encoding service
* Lauguage translation technologies

如何实现一个分布式优先级队列？

1. 基于 Thrift 通信
2. 有哪些 API 接口？
    1. Enqueue
    2. Dequeue
    3. Ack
    4. Nack
    5. GetActiveTopics

3. 通过 Shard Manager 管理分片的分布
4. 基于 Mysql 存储任务
5. 一个 Item 包含哪些字段？
    1. NameSpace
    2. Topic
    3. Priority
    4. Payload
    5. Metadata
    6. Dequeue delay
    7. Lease duration（每个任务个性化定制超时时间？）
    8. FOQS-assigned unique ID
    9. TTL

6. Topic 的含义（动态的，轻量的）
7. Namespace 的含义（多租户划分，资源隔离）
8. 一个任务如何插入队列？
![7b87b62f2e5b5e98a3f90dcbc11678b9.png](evernotecid://7F01B206-A616-4EA4-AABF-0C5FA9B75355/appyinxiangcom/26825047/ENResource/p413)

9.一个任务人如何出队？（预取的时候会更新数据库中对应行的状态，以防止多次交付，如果 FOQS 宕机，如何恢复这部分装填呢？）
![54dec0c08c5089b98fe78e975a32bd75.png](evernotecid://7F01B206-A616-4EA4-AABF-0C5FA9B75355/appyinxiangcom/26825047/ENResource/p414)
* 维护一个预取队列
* 每个分片按照优先级维护一个主键队列


目前的架构：
![32def38e83fe4d9b623f2302d9966e03.png](evernotecid://7F01B206-A616-4EA4-AABF-0C5FA9B75355/appyinxiangcom/26825047/ENResource/p415)

灾备：

1. 多可用区异步复制
2. 同可用区不同机房同步复制

分布式优先级队列本身更像是一种消息队列，基本保持和消息队列同样的功能。投递、消费、持久化等等。FOQS 直接基于 Mysql 来实现，相对于一些自己实现持久化的产品来说，开发周期应当更短。文章中对于 FOQS 的可扩展性提到的比较少，类似于 Topic 在 Shard 上的分布。
