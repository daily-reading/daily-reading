## Modern storage is plenty fast. It is the APIs that are bad.
[原文地址](https://itnext.io/modern-storage-is-plenty-fast-it-is-the-apis-that-are-bad-6a68319fbc1a)
传统存储架构里保留了对于 I/O 操作的一些假设，或者说偏见。在底层存储的读写能力大幅提升的今天，建立在这些假设上的架构无法发挥存储的最优性能。文章 focus 在读优化方面。

### 已有的假设
* I/O is expensive. Operations like page fault, interrupts, copies or virtual memory mapping are cheaper than I/O.

对于顶级 NVME 设备，I/O 的成本降低到了 single-digit microseconds 级别，已经和以上操作一个量级了。为了避免等待一次 I/O 而转求以上操作带来的收益可能微乎其微。
> 问：对于普通 SSD 呢？对于 SSD 而言，随机读写的性能差异与顺序读写的差异远没有机械硬盘那么显著了，毕竟 SSD 的寻址几乎是零成本操作，剩下的差异只有 block size 和 发送解析指令次数的影响了。I/O 能力方面，手上的 Samsung 960 的 IOPS 大约在 400, 000 左右，远超机械的一二百 IOPS 了，响应的读写延迟也是很低的咯。

* Read amplification. OS 默认会进行 read ahead 操作。

对于随机读，这样的 read ahead 操作是浪费的；对于顺序读，这种 read ahead 受限于 OS 的默认策略，无法保证最优性能。

* 传统API没有有效利用并行。

例如系统中只有读一个大文件的需求，期间线程除了等待这一个单一的读线程之外无事可做。
> 现代 OS 的核心能力就是处理多任务，正常场景下很少有所有工作被读一个单一大文件阻塞的需求吧。不过不可否认的是针对大文件读取速度的优化肯定是有意义的：尝试修改读取模式，倾斜更多资源来处理大文件读取的需求。

### 作者的优化
文中作者基于 io_uring 自己搞了套缓冲区，page_size 动态可调，然后加上了一套可自定义规则的 read-ahead 操作，在大文件读取场景下，相较于传统 API 效果显著。随机读方面，虽然 Direct I/O 慢了20%左右，但是内存占用几乎为0，也不好一刀切的说孰优孰劣吧。
结论是对于基于新型存储系统，所有读请求都要从存储中预加载到用户缓冲区这个传统行为，不一定适用。对于读操作敏感的场景可以考虑优化 API 了。

#### 附
文中提到 mmap-based systems，mark 一篇[中文博客](https://www.cnblogs.com/huxiao-tee/p/4660352.html)。之前面试被问到过这个，回答的不是很好。回过头来看，其核心就是少了一次内核态到用户态的数据拷贝，但是对于频繁写热点数据以及数据量太大频繁触发页替换时，mmap 可能就不好用了。MongoDB 和 rocketMq 中有用到 mmap。