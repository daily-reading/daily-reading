## Understanding Linux CPU Load - when should you be worried?
> 原文地址：<https://scoutapm.com/blog/understanding-load-averages>
- 本文以车过桥为例解释 Linux CPU load average
1. 单核CPU相当于一座桥，以1.0为界限，1.0对于桥上刚好车满了，大于1.0说明有堵车，小于1.0说明桥上还有空间，车即来即走
2. 通常我们会留一些余量将阈值设置为 0.7，大于 0.7 时就需要关注了
3. 多核就相当于多座桥，此时 1.0 应当乘以相应的倍数，3.0 对于 4 核是OK的
4. 双处理器四核、四处理器双核、单处理器八核 可以认为是一样的
5. 显示的三个值分别对应前一分钟、前五分钟、前5分钟的平均值
6. 查看 CPU 核数： cat /proc/cpuinfo 统计处理器个数：grep 'model name' /proc/cpuinfo | wc -l