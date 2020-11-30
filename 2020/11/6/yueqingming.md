**数据库读写分离之后导致的问题**
* 主从复制延迟将丢失写后读的一致性


**两种处理写后读一致性的解决策略**
* 在写入服务器时等到所有副本同步完成再响应
* 延迟读取，直到副本同步完成

**通用的写后读一致性方法**

1. 从 Master 获取当前 binlog 的偏移位置
2. 轮询副本的偏移位置，直到偏移位置超过从 Master 获取到的偏移位置或者超时为止
3. 如果找到同步成功的节点，则执行查询请求，否则请求失败。

**问题**
* 每次读请求前都对 Master 查询位置偏移，Master 的压力是否会过大
* 轮询副本，副本是否会承受更多的压力

不需要整体同步，只需要请求所对应的偏移量同步即可

**尝试的实现**

[How We Learned to Stop Worrying and Read from Replicas](https://medium.com/box-tech-blog/how-we-learned-to-stop-worrying-and-read-from-replicas-58cc43973638)
