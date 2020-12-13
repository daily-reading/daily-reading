## Cache coherency primer

### 基本概念

CPU 无法直接访问物理内存，是因为没有直连物理内存的线缆，需要通过 CPU 的 L1 缓存在访问。

20 年前，CPU 的 L1 缓存可以直接访问物理内存；但时至今日，需要通过更多层级的缓存，才会访问物理内存。

缓存用 ”行“ 来组织，一行的大小与 CPU 架构相关，有 32B、64B、128B 几种，每行与物理内存相对应。

内存访问流程：
1. 访问 L1D$，没有则从物理内存中载入（或从 L2D$ 等下层 cache 中载入）；
2. 如果有，则直接从 L1D$ 中返回，访问速度快；由于是一行一行载入，访问相邻内存地址速度也会变快；


性质一：基本不变性
```
cache 中的内容与物理内存的内容保持一致。
```


缓存写入
1. write-through：逐级写入 cache level 或直接清除 cache level 中对应内容。能保证上述基本不变性；
2. write-back：直接修改 cache level 中内容，并将其标记为 dirty，然后触发回写到物理内存或其他 cache level 中；

性质二：回写不变性
```
触发回写后，cache 中的内容与物理内存的内容保持一致。
```


回写是一种弱约束，保证回写结束后，cache 与物理内存一致。

单 CPU 用法：write-through、write-back 或 二者混用，例如：L1D$ 使用 write-through；L2D$ 使用 write-back，虽然复杂但高效，也有很多其他类型的优化方式。


### Coherency Protocol
—— 多核情况下的缓存读、写问题

问题：多缓存，到底该访问读、写哪个缓存？

Coherency Protocol：使得多个缓存看起来像一个缓存。Coherency Protocol 确保多个缓存的协同：具体应该读、写哪个缓存，需要所有缓存仲裁。

#### 方案一：Snooping Protocol，监听协议

实现：多个 CPU 监控共享总线上发生的缓存读写事件，进而保持 CPU 内部缓存内容同步。
总结：这种 CPU 缓存协同方式适合于 write-through 方式的缓存读写，但不适合 write-back 模型的更新方式。


#### 方案二：MESI Protocol，状态流转协议

实现：缓存维护一个状态标志，分为如下几种类型：
1. I - Invalid：不可用状态，缓存行过期或未载入；
2. S - Shared：共享状态，多个 CPU 缓存共享内容，多个副本内容一致；
3. E - Exclusive：排他状态，只有 E 状态的 CPU 缓存可被修改，其它缓存中相同内容状态均为 I，即不可用；
4. M - Modify：修改状态，表示缓存内容被修改，其它缓存中相同内容状态均为 I，即不可用；
E 状态解决了 “修改缓存内容前需要通知其它 core” 的问题。当其它 core 想读 E / M 状态的缓存行时，E / M 状态将转换为 S 状态。


#### 缓存顺序一致性的保证
1. 当缓存收到总线传来的读写事件时，立即做出响应；
2. 核心按顺序地将进程的操作发送给内存，并等待其结束；

但实际应用中，CPU 无法保证上述条件：
1. 缓存不能立即响应总线的读写事件；
2. 核心不能按顺序发送进程指令；
3. 存储数据是两段的：写入缓存、写入物理内存；

实际情况下，允许 CPU 读取到过期数据，不保证进程操作按顺序执行，具体实现分为两个阵营：
1. 软限制：CPU 不限制上述情况（读过期内容、按顺序执行指令），由程序员通过写屏障来保证，如：arm 架构；
2. 硬限制：CPU 追踪内存操作，当有操作异常时，会回滚操作并抛出异常，如：x86 架构；


### 总结
本文介绍了不同系统架构在缓存协同问题上的解决方案：
1. x86 架构做了更多的限制与保证，让程序员能够减少编码负担；但同时，增加了复杂度与能耗，导致性能不能达到极致；
2. arm / power 等弱限制架构，不做过多的限制，性能更佳；但需要通过额外手段确保程序执行的顺序、解决缓存脏读的问题等；
简单地窥探到了系统内部 CPU 缓存协同的秘密，收获良多。

### 额外

借着部分疑惑读了另一篇文章：[Why Is Apple’s M1 Chip So Fast?](https://debugger.medium.com/why-is-apples-m1-chip-so-fast-3262b158cba2)

通俗易懂地讲解了 M1 芯片为什么快，以及为什么 Intel、AMD 短期内无法赶上的原因。

**为什么快？**
1. M1 集成了 CPU、GPU、NPU（神经网络处理单元）等强大的专用处理器，可以定向提升视频渲染、机器学习算法的处理能力，所以快；
2. CPU、GPU 共享 Unified Memory，无需拷贝成本；
3. ARM 指令只有 4 比特长，M1 有 8 个解码器，可以并行解码 RISC 机器指令；

**为什么 Intel、AMD 短期内无法赶上？**
1. 苹果控制整个产业链，而 intel、AMD 等公司的芯片需要集成在主板上，要兼顾下游厂商，有职责边界问题；
2. x86 指令是变长的，从 1 比特到 15 比特不定，指令地址需要逐个寻找，无法并行解码，影响速度；


### 参考资料：
- [Cache coherency primer](https://fgiesen.wordpress.com/2014/07/07/cache-coherency/)
- [Why Is Apple’s M1 Chip So Fast?](https://debugger.medium.com/why-is-apples-m1-chip-so-fast-3262b158cba2)
- [What Does RISC and CISC Mean in 2020?](https://medium.com/swlh/what-does-risc-and-cisc-mean-in-2020-7b4d42c9a9de)
- [苹果电脑为什么要换 CPU：Intel 与 ARM 的战争](https://www.ruanyifeng.com/blog/2020/06/cpu-architecture.html)