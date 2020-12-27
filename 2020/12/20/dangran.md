https://scoutapm.com/blog/understanding-load-averages

uptime 命令 
- [20:14:33 up 460 days,  4:24,  1 user,  load average: 8.15, 7.86, 7.73]
load average 1分钟，5分钟，15分钟平均负载 

车桥模型

单核警戒线
* 70%  - 需要介入
* 100% - 有问题，需要立即响应
* 300% - gg

a load of 1.00 is 100% CPU utilization on a single-core box.

Multicore vs. multiprocessor

多处理器和多核处理器，粗略的来看，对load的敏感差别不大

* Rule of Thumb: 0.70 If your load average is staying above > 0.70, it's time to investigate before things get worse.
* Rule of Thumb: 1.00. If your load average stays above 1.00, find the problem and fix it now. Otherwise, you're going to get woken up in the middle of the night, and it's not going to be fun.
* Rule of Thumb: 5.0. If your load average is above 5.00, you could be in serious trouble, your box is either hanging or slowing way down.
* The "number of cores = max load" Rule of Thumb: on a multicore system, your load should not exceed the number of cores available.
* The "cores is cores" Rule of Thumb: How the cores are spread out over CPUs doesn't matter. Two quad-cores == four dual-cores == eight single-cores. It's all eight cores for these purposes.

