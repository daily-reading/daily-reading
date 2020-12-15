# Scaling Memcache at Facebook

* Author: Rajesh Nishtala, Hans Fugal, Steven Grimm, Marc Kwiatkowski, Herman Lee, Harry C. Li, Ryan McElroy, Mike Paleczny, Daniel Peek, Paul Saab, David Stafford, Tony Tung, Venkateshwaran Venkataramani
* Link: http://www.cs.utah.edu/~stutsman/cs6963/public/papers/memcached.pdf

Memcached 是一个非常简单的全内存分布式 KV 缓存系统，本文主要讨论了 Facebook 如何大规模使用分布式内存缓存来支撑其上繁重的网络请求。
