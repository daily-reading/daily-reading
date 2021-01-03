# Understanding Linux CPU Load - when should you be worried?

1. CPU负载界限
    - 0.7： 不用关心
    - 1：需要立刻去解决
    - 3：非常严重，要么挂起要么特别慢
2. 核数与负载是线性累加的：即 两个四核==四个双核==八个单核
3. 需要关心的CPU多长时间的平均负载

   > 使用`uptime`命令：
   >
   > ```23:05 up 14 days, 6:08, 7 users, load averages: 0.65 0.42 0.36```
 
 上面三个值分别代表最后1分钟，最后5分钟，最后15分钟的CPU负载值；
 我们应该关心5分钟的和15分钟的；