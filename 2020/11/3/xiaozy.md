# Cache is the Root of All Evil

> 原文地址 https://medium.com/box-tech-blog/cache-is-the-root-of-all-evil-e64ebd7cbd3b

1. 缓存能够有效地降低延迟和负载，但是带来一系列难搞的问题
2. 如果不加缓存就能满足性能要求，就别加缓存
3. 关注写时缓存失效、读时更新缓存的场景
    ![](https://miro.medium.com/max/700/1*ADUvdnyTltZmpubw7w3FPg.jpeg)
4. 一致性问题
    1. 读流量高时，一次写操作可能会导致众多读操作去读系统记录（惊群效应），来填充缓存
    2. 并发读写可能会导致缓存值一直是无效的
        - 读操作读出记录更新缓存之前，发生了写操作，导致缓存失效
            - lease 租约
                - 基本操作
                    - `atomic_add(key, value)`: 当且仅当 key 不存在时设置值
                    - `atomic_check_and_set(key, expected_value, new_value)`：Compare and Set
                - 读操作：
                    1. 查询缓存，如果缓存 Hit 则转 2，如何 Miss 则转 3
                    2. 如果 value 是 lease，则转 sleep 一段时间后转 1，否则直接返回缓存值
                    3. Cache Miss 时先尝试通过 atomic_add 添加一个 lease，成功后转 4，若失败重新尝试读 Cache，即转 1
                    4. 读系统记录，通过 tomic_check_and_set 更新缓存，并返回记录
                ![](https://miro.medium.com/max/700/1*WBXevf7GWzV4NwypmIDotg.png)
                - 这个方案有效的解决了惊群问题
5. 如果多个写操作频繁发生，会导致读操作变成串行，从而 latency 升高或超时
    - 解决办法
        - 任何一个等待租约的读操作，可以使用获得租约的读操作读取的值，即使这个值后面被写操作失效
        - 这个方法需要在缓存的另一个位置保留租约读的结果
        ![](!./'Cache Read With Previous Seen Lease.png')
    - 这个方案的一些问题
        1. 为什么要优先返回 valueForPreviousrlySeenLease，这样不是会优先读到旧的值？
            - 首先，只有当上次看到的 valueForKey 是 lease 时，valueForPreviousrlySeenLease 才会有值，说明上一次读时，有其他 reader 设置了 lease，并处于更新 vlaue 过程中
                - 当前 reader 上一次读要么 valueForKey = null 但是 add lease 失败了，要么看到了其他 reader 的 lease
            - 对于其他 reader 的lease，本次读有几种情况
                - 没有新的写操作发生
                    - 获取 lease 的 reader checkAndSet 成功了，则不会有新的 lease 产生，根据上次看到的 lease 读到的值就是最新的，和 valueForKey 值应该一样
                    - 获取 lease 的 reader checkAndSet 失败了，lease reader 也会将其读到的值写到 cache 里，因为 addLease 是原子操作，addLease 成功时才会尝试 checkAndSet，不考虑网络问题，只有当又发生了其他写操作时才会导致 checkAndSet 失败，这种情况不会发生
                    - 如果 lease reader 因为网络问题 checkAndSet 失败了， 执行 cacheCluster.set(lease, valueFromSourceOfTruth)
                        - 如果 cacheCluster.set 成功，则之后每次都会从 lease -> value 中读到正确的值
                        - 如果 cacheCluster.set 也失败，则会陷入死循环，知道有写操作，导致 key 里的 lease 失效（待确认）
                - 有新的写操作发生
                    - previouslySeenLease reader 写的 lease -> value 还在，但是 valueForKey 已经没了，这时候如果再重试会导致串行读，这里直接返回是牺牲了一致性，读到了过期副本，但是保证了读操作的吞吐量
                    - 写操作之后到来的读操作，会看不到这次 lease，会读到 valueForKey = null，从而尝试将 value 更新到最新
