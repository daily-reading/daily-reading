原文：https://scoutapm.com/blog/understanding-load-averages

* 常用命令，top、uptime
* Load AVG 3 个数值，分别为 1 分钟、5 分钟、15 分钟
* core 与 load 成正比，1 core 能承载 1.0 load
* 1 core，load 最好保持在 0.7 下
* 排查问题时，多关注 5 分钟和 15 分钟的 load AVG

**CPU load**
>This  is basically what CPU load is."Cars" are processes using a slice of CPU time("crossing the bridge") or queued up to use the CPU.
* cpu load  = 是正在执行和等待的进程