原文：https://eng.uber.com/m3/

## M3 结构
![M3结构](http://1fykyq3mdn5r21tpna3wkdyi-wpengine.netdna-ssl.com/wp-content/uploads/2018/08/image4-1.png)

## Metrics before M3
### 现状
* 所有指标发送到 Graphite stack
* 使用 Whisper 文件格式存储
* Grafana 作为大盘
* Nagios 用来报警

### 问题
1. 扩展 Carbon 集群需要手动重新分片
2. 单点问题

### 需求
1. 提高可靠性和可伸缩性
2. 支持全局查询
3. 低延迟，保证查询大盘速度和报警可靠
4. 支持灵活、带标签的数据模型
5. 向后兼容

## Introducing M3
### 优化
* 优化 metrics pipeline，减少内存使用
* 保证数据高度压缩
* 因为数据特点是，很多数据写一次，从不读。因此每个 shard 维护一个 Bloom filter 和 index 是summary
* 避免 compaction，引入 downsampling path，从而提高主机资源利用率和稳定的读/写延迟
* 本地设计存储 TS

### 实现
1. Global View:  replication 是按 region 划分的。查询时会 fan out 到所有 region，返回压缩值。未来将在每个 region 先聚合，再返回
2. Fewer Compactions: M3 在采集时进行采样
3. Storage policies, retention, and downsampling：让用户选择他们想要长期存储到 metrics，根据标签决定存储时间
