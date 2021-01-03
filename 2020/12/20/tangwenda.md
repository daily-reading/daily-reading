# Understanding Linux CPU Load
[原文地址](https://scoutapm.com/blog/understanding-load-averages)
## CPU load defenition
- 拿过桥来类比，桥上的车辆等待数 / 桥的桥面容量可以用来量化桥梁的拥堵程度
- 对于单核 CPU，可以理解为 CPU 的事件处理队列的占用程度就是 CPU load的有效量化值
- 对于多核 CPU，queue的数量变成了单核 CPU 数 * 核数，CPU load 的除数依然不变，
  所以 load 除以核数才等于单核的实际负载情况
  
## 警戒值
平均到每个核，拉长时段到15分钟平均值，一般有这么几个经验性的警戒值
- 0.7 以下，正常。此时仍有 buffer
- 0.7 - 1 需要关注，考虑怎么降低负载
- 1 - 5，fix it NOW
- 大于5，GG 不知道还有救没

对于瞬时性的波动，比如一分钟平均，短暂的飙到1还可以接受。不过对于服务而言，
  响应延迟可能就上去了；对于数据库而言应该要出现慢查了。