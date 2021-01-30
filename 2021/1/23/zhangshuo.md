原文：https://martin.kleppmann.com/2016/02/08/how-to-do-distributed-locking.html

### 问题
client1 获取锁后，由于 GC 或 page fault 等停顿导致租约过期，但自身无感知。此时用 client2 拿到锁并修改数据，client1 结束停顿后写入数据，最终数据不一致。

### 解决
#### Making the lock safe with fencing
1. 乐观锁，用户 1 拿到锁同时拿到 token，token 单调增，存储层拒绝 token 倒退的写操作
2. 可以使用 zk 的 zxid 或 znode 来作为 fencing token
3. redlock 无法生成单调增的 token

#### Using time to solve consensus
1. 算法通常确保安全属性不做时间假设，即，系统中的时间不一致，算法性能会下降，但不会做错误决定
2. redlock 依赖很多时间假设，假设所有 redis 节点都能对同一个key 在其过期前持有差不多点时间，网络延迟相对于过期时间很小，进程暂停时间相对过期时间很小

#### Breaking Redlock with bad timings
举例证明 redlock 因为时间问题而不可靠
##### 例子1
1. client1 从 A,B,C 节点拿到锁，D,E由于网络问题没有到达
2. C 节点时钟 jump forward，导致锁过期
3. client2 在 C,D,E 节点拿到锁，A,B 由于网络问题没有到达
4. 最终 client1 和 client2 都拿到锁了 
##### 例子2
1. client1 从所有节点拿到锁
2. 获得锁的 response 还没到达 client1，client1 就停顿了
3. 停顿时间内锁过期
4. client2 从所有节点拿到锁
5. client1 停顿完后收到 response，于是拿到锁
6. 最终 client1 和 client2 都拿到锁了

#### The synchrony assumptions of Redlock
只有假设一个同步模型，redlock 才能正常工作
1. 有限的网络延迟，数据包总是在某个最大延时之内到达
2. 进程停顿边界，进程停顿一定在某个最大时间内
3. 时钟错误边界，不会从一个坏的 NTP 服务器取得时间

### 结论
不建议使用 redlock
1. 对于需求性能的分布式锁应用，redlock 太笨重，成本也高
2. 对于需求正确性的应用，不够安全