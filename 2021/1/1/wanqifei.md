# How Google Spanner Assigns Commit Timestamps - The Secret Sauce of Its Strong Consistency

* Author: Eileen Pangu
* Link: https://levelup.gitconnected.com/how-google-spanner-assigns-commit-timestamps-the-secret-sauce-of-its-strong-consistency-8bc143614f26

介绍了 Google Spanner 作为一个全球分布式数据库，是如何使用 TrueTime API 解决全球强一致性问题的。

## 为什么准确的提交时间戳很重要?

分布式系统的根本挑战在于, 对于有状态转移的系统分片，其实很难保证它们之间的同步一致性。产生这个问题的原因:
- 系统组件可能因为频繁且无法预测的故障发生不同步的情况。
- 从系统设计考虑，如果每次系统的状态变化都需要让系统中所有组件同步进行，无疑会极大降低系统性能。

如果有准确的提交时间戳，则可以为系统带来有序性。当所有的事件都带上了其发生时的真实时间戳，系统会同时根据它的提交时间和全局 wall-time 时间来决定是否更新，何时更新。
这样，一个系统才可能具备强一致性的能力。具体来说, 一旦一个写被提交了，之后所有的读操作都能被读取到，因为读操作的时间戳要大于写的时间戳。

不过应用于全球的精确时间戳是非常难的。首先不能依赖机器的本地时钟，因为时钟偏移在分布式系统中是真实存在的。研究表明，一个本地时钟可能会在短短30秒内漂移约几毫秒
这对于一个由数万台机器组成的全球分布式系统来说是灾难性的，因为这些机器希望使用时间戳来进行事件排序。Spanner通过构建TrueTime API来克服这个问题。


## 什么是提交时间戳?

> 提交时间戳是一个事务在获得这个事务需要的所有锁之后、释放这个事务占据的任何锁之前分配给它的时间戳。

数据库事务依赖**2阶段**提交来保证事务隔离性。如果两个事务在争夺锁，在一个事务没有释放锁之前，另一个事务无法获取任何锁。
因此，分配一个在此期间的时间戳，这个时间戳就能反映出它们的执行顺序和因果关系。


## Spanner 的 TrueTime API

调用者调用 TT.now()。TrueTime API 返回一个介于 `[earliest, latest]` 的时间戳。一般来说这个区间是在毫秒级别的，谷歌在每个数据中心都部署了 GPS 和原子钟，这样可以让
出现误差的概率仅仅比 CPU 故障概率要高6倍而已。


Spanner的数据按表键进行分区，以实现可扩展性。然后，每个分区都会被复制，以实现冗余和性能。在每个分区中，在 replica 中选出一个 leader。所有的写入都要经过 leader。Spanner使用multi-paxos来实现基于时间的 leader 租约，因此一个分区（leader+replicas）被称为paxos组。当 leader 租约到期时，该组将选出一个新的 leader。领导者可以通过发送 "心跳 "来延长其租约。一个事务只有在其所有的读写都涉及到一个分区的数据时，才会与该分区进行交互。

当一个事务涉及多个分区时，会发生分布式事务。spanner客户端库从参与的分区中选择一个 coordinator，并开始一个2阶段的提交。最终，当 coordinator 收集到其他分区的必要信息后，它将分配时间戳并通知参与者提交数据。


## 提交时间戳分配方式


### 单个分区事务提交
在单一分区的情况下，分区 leader 需要确保提交时间戳大于之前分配的所有提交时间戳。具体实现如下:

![single-partition](https://miro.medium.com/max/1400/1*GrYJyUSezNqsmVtOSjAPVQ.png)

可以看到，通过对时序的控制，Spanner 分配给下一个事务的时间戳一定是大于之前的，而在等待时间戳分配时，Spanner 同时会进行一些跨州数据复制的工作。

### 跨多分区事务提交

在跨多分区的情况下，coordinator 需要确保提交时间戳不仅大于自己之前的提交时间戳，也大于其他参与者之前分配的提交时间戳。coordinator 在 2 阶段提交的过程中会收集这些信息。
在2阶段提交的 prepare 阶段，coordinator 会收到参与者的回复。参与者可以在这些回复中发送他们之前的提交时间戳。最后，coordinator 会指示其他参与者提交，其中也可以传达所选的提交时间戳。

示意图如下:

![multi-partition](https://miro.medium.com/max/1400/1*I9kavh5xg0TweJoOqaFl0w.png)

1. coordinator 首先获取锁并生成自己的提交时间 `C_a`，然后等待其他分区参与者交换时间戳信息
2. 其他分区参与者获取锁，并生成提交时间 `C_b` 和 `C_c`，并将其发送给 coordinator
3. coordinator 选择其中最大的时间戳作为该事务的提交时间戳，并提交事务。
4. 在可以安全释放锁时，coordinator 会通知其他分区参与者。
5. 其他分区参与者释放自己获取的锁

一个新的事务可能会在 coordinator 释放锁以后立即开始，尽管其他参与者可能还在锁中。如果是 coordinator 单分区的事务，则没问题，如果是跨分区的，需要等待其他分区参与者完全释放锁。


