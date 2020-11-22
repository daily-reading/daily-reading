
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
        * File region的状态取决于 修改操作的类型、成功与否、是否是并发修改
        * 定义file region一致性：所有client能够看到相同的数据
        * 写操作都会是 write 和 record appends


系统交互
后面继续


