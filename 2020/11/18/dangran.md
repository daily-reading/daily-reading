
目标：
* 满足google高速增涨的数据需求
* Performance, scalability, reliability, availability
* 满足google自己的应用环境的工作流和技术环境，包括当前的和未来预期的

对此文件系统的假设
* 组件的实效是常态（包括磁盘、内存、网络、供电啥的）
* 假设文件都是很大的，需要高效支持GB级文件，也要支持小文件，但不需要额外优化
* 大多数的文件都是 追加写，而不会更改已有数据 (appending new data rather than overwriting)。会有更改已有数据，但不需要额外支持太多
* 负载由两类读组成：顺序读和随机读
* 需要支持多个client 并发修改一个文件
* 高带宽比低延时更重要

接口
没有支持常规的 POSIX 接口，提供了基本的 create, delete, open, close, read, write 操作，还提供了非常规的 snapshot, record append提供复制快照和并发修改的操作

架构

* Single-master + multiple-chunkservers
* 基于linux，用户级别 进程
* 一个文件分为若干 固定大小的 chunk，由全局唯一的 chunk handle 标识，一般会有若干副本
* master
    * 负责 metasspace，包括 namespace, access control, mapping for chunks, location of chunks
    * 还负责chunk的释放、垃圾回收和在chunkserver之间迁移
    * master和chunkservers之间通过心跳进行通信
    * 单主简化了设计，允许主节点做全局的，chunk的位置、副本的决策
    * 需要避免master成为性能瓶颈，master不承载任何数据交互
    * client和master通信，问应该访问哪个chunkserver
    * master缓存信息
* client
    * metadata相关和master交互
    * 数据相关直接和 chukservers交互
    * client不做缓存，因为太大了
* chunk
    * 64mb 
    * 普通的linux文件
    * 懒加载分配空间
    * 这么大的好处
        * 减少client向master请求chunk的次数
        * client一次可以对一个chunk做很多操作，减少网络压力
        * 减少master的 metadata

* metadata
    * file and chunk metadata
    * mapping from files to chunks
    * Location of chunks
    * 内存内数据
        * 所有metadata都会存在一份mater的内存中
        * chunk location
    * Op-log
        * 保存所有metadata 的变更记录，是唯一的持久化metadata数据
        * 维护起逻辑时间线
* 一致性模型
    * 相对轻松、简洁的一致性模型
    * file namespace的变更（创建等）是原子的，由master用锁进行控制
    * file region的一致性讨论
        * 定义 defined: consistent & client 能看见 它 的写入完全成功 (consistent: 所有client看到同样的数据)
        * File region的状态取决于 修改操作的类型、成功与否、是否是并发修改
            * 写操作都会是 write 和 record appends
            * 如果一个写入操作没有并发的干扰的话，则是defined的（这个比较好理解）
            * 如果一个并发写入，会让写入的region 处于一个 undefined but consistent 的情况: 所有client看到同样的数据，但是写入操作不一定是完全的


系统交互
Lease And Mutation Order
* 修改应用于所有的副本
* 用Lease来管理多副本管理的一致性
* 由master下发，并管理顺序
* 写流程
    * client询问哪个chunkserver 持有此chunk lease 并且询问副本位置。若无lease，则下发
    * master回复 1.location, 2.chunk primary 标示
    * client push to all replicas. 无序，chunk server 有 LRU cache. 解耦数据流和控制流，可以使得我们单独基于拓扑关系优化数据流
    * 当所有副本都接收到了数据之后，client send a write request to primary
    * Primary 发送写入命令到所有 二级副本
    * 所有副本 ack primary 写入成功
    * primary 返回 client 具体的写入状态，如果有失败，则可以走重试流程 


如果文件过大，client 会 拆分成多个副本

* 数据流 (这里有点没搞明白)
    * 为了利用每个机器的带宽，数据是线性的上传到一系列的chunkserver，而不是分散的用一定的拓扑目标上传？
        * 这里我的理解是，client的上传带宽，一次只给一个chunkserver 上传？(pipeline)
    * 这里我的理解是，在通过ip计算逻辑距离的网络拓扑中，实现了类似于P2P的上传路由？
        * 比如S1 —> S4, 可能先从 S1 —> S2, S2 —> S4
        * 这里避免 单纯的将 数据发到 未收到数据的 最近机器节点
        * 而是通过 物理上最接近的拓扑图，一层一层的传递，有点像 torrent 协议里的寻址？
        * 所以对于client而言，只会上传一个完整数据
    * B/T + R * L，B是传输文件大小，T是网络出口带宽，R是副本数量，L是latency(很小)
* Atomic record appends
    * 多个client 修改同一个文件 ,  多producer, 单consumer模型
    * 同样的控制流，在primary有了少许额外逻辑，而不是通过client distributed lock 管理
    * 在client push 最后一片数据到所有的副本之后，发送request 到 primary
    * primary判断chunk是否会超过64m的限制
        * 若超过，则让client 做重试 上传到下一个chunk
        * 一次记录追加操作最多只能 1/4 chunk 大小(16m)

主节点操作
未完待续...


