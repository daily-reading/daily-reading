# Cache coherency primer
## Cache basics
- 先明确避免讨论memory-mapped IO and write-combined memory。感觉有时候我介绍一件事情的时候修把所有case都照顾到，导致一瞬间信息量太大。
-  the cache hierarchy
  - L1 L2 L3
- Caches are organized into “lines”  感觉这一段又写的很好，明确说明了自己有简化，一方面可以让文章更易懂，一方面又在提醒读者还有更深的复杂性，吸引读者去深入
> Basic invariant: the contents of all cache lines present in any of the cache levels are identical to the values in memory at the corresponding addresses, at all times.
- read-only access很容易遵守
- 加上写操作就复杂多了
  - Write-through  just pass stores through to the next-level cache (or memory)
  - Write-back  doesn’t pass writes on immediately  mark dirty
  - 为什么会有write-back  
    -  filter repeated writes to the same location
    -  it can issue one large memory transaction instead of several small ones, which is more efficient.
    
## Coherency protocols
- 多核CPU如何share cache
  - there’s only one L1$, and all processors have to share it
    - too slow
    - 问题的根本不是muti-core，而是muti-cache
  - have multiple caches and then make them behave as if there was only one cache.
- “snooping” protocols
  - all cores knows all traffic on a shared bus. Only one cache can perform memory transcation at any given time. All other caches snoop and knows this traffic, and will determine if its own data is staled after the transcation.
  - work fine for write-through caches，BUT NOT for  write-back 
  - therefore, we need to broadcast not only when data changes happens, but also when we intend to change our local copy of data.

## MESI and friends
- Invalid lines
- Shared lines 
  - clean
  - can’t be written to
- Exclusive lines
  - clean
  -  the same line must be in the I state in the caches of all other cores.
  - This state solves the “we need to tell other cores before we start modifying memory” problem 像是一个锁
- Modified lines
  - dirty
  - must be in the I state for all other cores
- Cases where sequential coherency might break:
  - Caches do not respond to bus events immediately.
  - cores do not in general send memory operations in strict program order
  - "store buffer"
- 所以很多代码都需要自己加入memory barriers，通过程序保证。 Architectures with a weak memory model
- weaker memory models make for simpler (and potentially lower-power) cores. Stronger memory models make the design of cores (and their memory subsystems) 这难道就是苹果要用ARM架构的原因吗，同等功耗效率更大

## 小结
- 感觉这篇文章很好的地方在于，把一些简化的点都显示说明出来，不会产生误导
- 明白如何简化模型会让人更加容易懂，并且信息传递丢失更少
- 期待下一篇
