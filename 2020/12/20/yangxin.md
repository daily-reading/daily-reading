# Understanding Linux CPU Load - when should you be worried?
- 怎么去理解 load average 的真实含义
- 用桥上交通的比喻来理解 Linux CPU load average，排队过桥的车辆就好比进程
  - 0.00 means there's no traffic on the bridge at all
  - 1.00 means the bridge is exactly at capacity
  - over 1.00 means there's backup
  - 2.00 means that there are two lanes worth of cars total 
  ![avatar](https://cdn.buttercms.com/uFU3AC14SzytfHXTy6WX）
- 应该关注最近 15 min 的数据，暂时的峰值可以容忍
