# Cache coherency primer

> 原文地址 https://fgiesen.wordpress.com/2014/07/07/cache-coherency/

1. 现代 CPU 都有多级缓存，例如 L1 L2 L3 缓存
2. CPU 写缓存的两种方法
    1. Write through: 将写入传递给下一级缓存或者内存，**缓存中的内容和内存中的内容总是一致**
    2. Write back: 写操作先写本地的缓存，然后将缓存行（Cache Line）标记为 dirty -> Dirty Cache Lines trigger write back，将变更写到下一级缓存或是内存 -> write back 完成后 cache line 变成 clean，脏缓存行被淘汰时必选先 write back，**缓存中的内容并不是总和内存中数据一致**
        1. 优点
            - 同一个位置的写操作能被过滤掉
            - 大多数写操作同时 write back，一次写的内存更多，更有效（因为 CPU 速率远大于内存）
3. CPU 缓存一致性协议（Coherency Protocols）
    1. snooping: 
        1. 基本原理
           1. 所有的内存操作都发生在一个共享总线上，能被其他核看到
            1. cache 是互相独立的，内存由所有核共享
            2. 访问内存操作需要裁决，在任意一个时钟周期内只有一个 cache 能够读写内存
            3. 当 cache 代表 core 来读写内存时，通知所有的核，允许所有核同步缓存。所以当写操作发生时，其他核知道缓存失效
        2. 特点
            1. write-through 很容易实现
            2. write back 不容易实现，因为 write back 不是写 cache 后立即执行，可能会导致冲突，因此写操作前需要先告知其他核
    2. MESI
        1. 基本原理
            1. 缓存行的四种状态
                1. **I**nvalid：不存在缓存内容或者内容已失效
                2. **S**hared：对主存内容的干净副本（clean copy），多个核可以同时持有这个缓存行
                3. **E**xclusive：也是对主存内容的干净副本，但是一次只能有一个核持有
                4. **M**odified：dirty，modified locally，如果一个 Cache Line 是 M 状态，对于其他所有核都必须是 **I** 状态。M 状态的缓存行必须在失效(invalidated)或者被淘汰(evicted)的时候写回内存
            2. 任意一个核只能写 E 或者 M 状态的缓存行，来解决“写缓存行之前必须通知其他核”的问题
            3. 如果其他核想要读一个处于 E 或者 M 状态的缓存行，E 或 M 状态的缓存行将会被恢复成 S 状态，如果是 M 状态，将会先将变更写回主存
            4. 作为软件工程师应该知道
                1. 在一个多核系统中，读缓存行需要和其他核通信，从而导致发生写内存操做；写缓存行分为多个步骤，当想要进行写操作时，首先需要需要获取缓存行的排他性所有权（Exclusive Ownership）并复制已有的内容（Read For Ownership）
                2. MESI Invarient：将所有脏缓存行（M 状态）写入内存后，所有缓存行中的内容和内存一致。在任意时刻，只要一个内存位置被一个核排他性缓存，将不会被其他核缓存
            5. 如果引入 **O**wned 状态，即允许共享 dirty lines，则出现 MOESI 变种
            6. MESI 保证了访问内存的顺序一致性
4. 由于指令重排、invalidate queue 等，无法保证 CPU 的存储指令的顺序，需要通过适当的内存屏障（告诉哪部分代码不要进行制定重排）来保证
