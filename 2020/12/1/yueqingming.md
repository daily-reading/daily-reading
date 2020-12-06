* Lots of computing can be characterized as “append-only”. This
section looks at some of the ways this is commonly accomplished（数据不可变，通过追加来解决问题，事件溯源？）
* 日志追加（系统中的某个状态只是日志在某个节点的缓存）
* 变更记录的顺序追加？



* Data on Outside vs Data on the inside
    * Data on the inside
    * Data on the Outside
        * Is Immutable
        * Is Unlocked
        * Has Identity
        * Maybe versioned 
 
* Functional Computation with Immutable Inputs
* Failure and Restart Are Based on the * Idempotent Nature of
Functional Computation over Immutable Inputs

version 的两种形式：

* line(one parent and one child)
* DAG(many parents and/or many children)

LSM(Log Stuctured Merge tree):
LSM 的组成结构？
LSM 更多的表示的是一种思想，可以不同的实现。
http://www.bzero.se/ldapd/btree.html
https://www.cnblogs.com/yankang/p/11041173.html

为什么会逐渐发展出不可变数据？

* 储存成本降低
* 分布式锁控制成本更高
 
不可变数据提供了那些好处？

* 数据一致性

使用不可变数据可能遇到那些问题?

* 不可变数据导致写入时的数据快速扩张，增大读取压力。

什么是不可变性？

* Immutability does change everything!

