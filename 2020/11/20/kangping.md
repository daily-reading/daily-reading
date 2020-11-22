# Cache coherency primer

## 1. Cache basics
* Modern CPUs talk to L1 caches only. (L1 talks to L2 -> L3 ... -> memory)
* Data load instructions are executed by loading block of memory (cache line) at once. Size of cache lines is determined by your Chip instruction set. 

 Note: 

 1. This is why data locality is important in computing. A simple example would be: matrix multiplication A * B is faster, if one is physically stored as its transpose. 
 
 2. You would probably also want to minimize "cache misses" by use less branching in your code.

* Basic invariant: the contents of all cache lines in any cache levels needs to be identitical at all times.

* Write to cache is more complicated. You inevitably violates the Basic invariant. Mechnically, you either "cascade" your cache line changes to the next level right away: "Write through", or you keep the current changes and flag your cache line "dirty", then choose to "cascade" your "dirty" cache lines back to its memory location at a more suitable time: "Write back". For "write through", we still keep the basic invariant, but operation might not be optimal. "Write back" gives us more degrees of freedom to optimize, however, we can only enforce a weaker form of invariant: After writing back all "dirty" cache lines, the content of all cache lines in any of the cache levels need to be identical to the values in memory. 
* Both write-back and write-through models are used in model CPUs. They can even co-exists on the same chip.

## Coherency protocols
* Designs mentioned in first section is adequate for single core CPUs. However, something is missing when multiple cores with their own caches share the same memory. 
* It is "memory" or "data" coherency problem, so more of a "multi-cache" problem rather than "multi-core" problem. L1$ is designed to be located close to core for speed and typically not shared among cores. Exactly the type of situation where we need cache coherency protocols.
* Snooping protocol: all cores knows all traffic on a shared bus. Only one cache can perform memory transcation at any given time. All other caches snoop and knows this traffic, and will determine if its own data is staled after the transcation. 
* Snooping protocal is enough for "write-through", but not so for "write-backs". For "write-backs", the actual write time maybe way after when data was changed in caches, therefore, we need to broadcast not only when data changes happens, but also when we intend to change our local copy of data.
* MESI protocols
  1. Invalid cache lines
  2. Shared lines
  3. Exclusive lines. (When a shared line is owned by one cache, it becomes E line for this cache. And it becomes I lines for all other caches)
  4. Modified lines. (When a line is modified by one cache, it becomes I lines for all other caches.)

  Here only E-line is new for multi-cache CPUs. 
  
* Cache can only write to memory if in E or M states. When other cores wants to read from E or M line, it reverts back to S line. (This would involve writing M line back to memory first)
* Conclusion for MESI
  1. in a multi-core system, cores need to communicate before memory transcations, and it needs to acquire exclusive rights before write to memory
  2. We need MESI invariant: after writing back all M-lines, all lines in all cache are identitcal, and identical to values in memory, and a memory location is exclusively cached by one core.
  
* Memory models: ARM and POWER architerctures have weaker memory models compare to x86, mostly for performance reasons. 
* Cases where sequential coherency might break:
  1. Caches do not respond to bus events immediately.
  2. cores do not in general send memory operations in strict program order
  3. "store buffer"
 
* weaker models do not check, but allow developer to insert memory barriers. x86 keeps "Memory ordering buffer", so that it can roll back invalid operations when it detects a consistency-break. 
* There are obvious performance benefits when you waive those consistency checks. At the time of writing, x86 seems to be doing fine on the performance front, which might no longer be the case as of Nov 2020. 