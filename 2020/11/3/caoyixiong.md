# Cache is the Root of All Evil

## 作者对于Cache的态度
如果服务能够忍受在不使用Cache情况下的性能以及负载，那么就不要使用Cache，因为Cache瞬息变化的特性使得很难去Debug，就使的问题变得更加神秘。

## 主要内容
1. 缓存的更新机制由Reader发起，Writer只做写DB和remove缓存的操作
在此机制下，若出现并发读，则会引发惊群效应(大量读操作更新cache的情况)，所以文中提到了使用 租约 的方式进行处理
即每次读请求需要分成这几个步骤：
     1. 先判断某个key在Cache中是否存在value
     2. 不存在则通过`atomic_add`(即`Redis`的`setnx`)设置当前key为A租约，然后读库，通过cas的方式更新cache
         如果存在cache，并且当前cache是租约(说明有其他Reader了)，则等一会再重新拿
         如果存在cache，并且当前cache不是租约，则认为是正常的值，返回
2. 上面的的机制在大量并发读夹杂着写尖峰的场景下也可能出现Reader之间出现排队读的情况（因为一个Reader读到另外一个Reader的租约之后，需要等一会再拉cache）
所以文中又提出了一种方式：
    即当一个成功设置租约的reader触发的cas cache失败了，也依旧返回旧的值
    并且读到此上面reader的租约的其他reader也都返回相同的值
这样能够解决写后读的原因？
因为：如果一个reader读到了另外一个reader的租约，则这个reader一定是在writer remove cache之前触达的（如果之后到达的，就自己设置租约了），
     也就是在writer Ack之前，这样就不会出现写后读数据不一致的情况了；
  
## 总结
这个思路很巧妙，就是不太好理解。


`populated upon read`(读时更新) 相对于 `write-through`(写时更新) 的优势在于：
如果要写时更新，则需要进行自实现的一致性锁（保证缓存和db的一致性）。
   1.会增大事务时间，由此增加写并发冲突
   2.自实现的一致性锁 相比于 `Deletion of elements from cache on DB updates` 的风险性更高

    
