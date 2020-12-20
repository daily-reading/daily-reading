# [Understanding Linux CPU Load](https://scoutapm.com/blog/understanding-load-averages)

- 用桥上交通的比喻来理解 Linux CPU load average，排队过桥的车辆就好比进程
- 单核边界值 1 表示车满了，小于 1 则有空余可以随时过桥，大于 1 表示桥过载需要排队等待。通常会有个 0.7 的界限，小于 0.7 表示还不需要关注
- 对于多核，1 的单位要乘以相应的倍数。1 只是一条路一座桥的度量，多核就是多条路。所以 3 对于四核来说是 ok 的
- 多核场景下「 核 」是个上限，两个四核、四个双核、一个八核是一样的
- 一般 load average 有三个指标供参考，分别是一分钟、五分钟和十五分钟内的平均值，不同 case 下关注的点不同
