Linux 下 load average: 0.09,0.05,0.01
数值所代表的含义？
什么时候代表这个到了阈值？
CPU 核数决定了你的CPU Load 值为多少是正常的，每一个 CPU 核数可以为你的负载提供 1 的值。无论是单核处理器还是多核处理器，他们提供的处理器核数都是大致等效的。
峰值 CPU Load 到达 1 是可以接受的，只要不是持续。

如何查看 CPU 核数：
cat /proc/cpuinfo
统计处理器个数：grep 'model name' /proc/cpuinfo | wc -l
