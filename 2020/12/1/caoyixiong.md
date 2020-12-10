# Immutability Changes Everything

## 本文主旨
Immutability does change everything!
## 主要内容
1. 存储成本降低，而数据更新时的锁协调更难了；所以提出通过不可变的数据副本来降低锁协调的压力。
2. Append-Only Computing
   - Observations are recorded forever. Derived results are calculated on demand 
3. Changes are layered over their predecessors. New values supersede old ones. 每个更新操作本质上都是一次新增操作，即记录了数据在不同时刻的历史快照。
4. Outside Data & Inside Data
    - Outside Data (Document,File or Message) 
       - Is Immutable
       - Is Unlocked
       - Has a unique Identity
       - Maybe versioned : Updates aren’t updates but new versions with a new unique identifier
    - Inside Data (Relational Field)
      - Is Changeable
      - Is Locked
      - No Identity:Data by value
      - No versioning: Data by value
5. When we have snapshots or some form of isolation, database data becomes semantically immutable for the duration of the calculation.
6. Versions Are Immutable
   - Different Version 
     - A linear version history: One version replaces another. There's one parent and one child.
     - Directed acyclic graph of version history: eventual consistency. many parents and/or many children.
   - MVCC
   - [LSM(Log Structured Merge Tree)](https://www.open-open.com/lib/view/open1424916275249.html): 
     - LSM presents a facade of change atop immutable files.
7. Although the data set can be changed physically, it is semantically immutable
8. Immutability is the Backbone of "Big Data"
   - Failure and Restart Are Based on the Idempotent Nature of Functional Computation over Immutable Inputs
7. Immutable files and immutable blocks empower this replication Since the Replication doesn't pay attention to any update anomalies.
8. Widely Sharing Immutable Files Is Safe
9. SSD 通过COW的方式来进行物理块的磨损平衡
10. 不变性引起了额外的存储消耗；如果使用COW,则会引起数据的多次复制
## 总结
突然想起来之前有过对于课件更新的重构：最开始课件都是直接update，所以锁冲突非常严重，经常报错。后面就将课件定义为不可变的，每次的更新都是一次新增，并通过parentId进行关联，重构完之后冲突就基本没有了
 