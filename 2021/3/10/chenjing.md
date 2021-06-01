## An unlikely database migration
>  原文地址：<https://tailscale.com/blog/an-unlikely-database-migration/>

最初:使用SQLite -> 频繁更改数据库设计,拖累迭代速度

-> 单文件存json,易于测试(作者居然起了个名叫JSONMutexDB) -> 文件越来越大,读写越来越慢

-> 不使用 MySQL或PostgreSQL 的原因: 不熟悉其事物语义, 测试变得缓慢

-> etcd 一个优秀的KV数据库,原因:适配业务逻辑,分拆大锁,减少IO;问题:用户少,需要自己读源码改bug,但代码量并不大

-> tailetc 在客户端封装一个client进行缓存,每次数据库的更改会被推到客户端,这样查询时无需网络流量

问题: nosql数据库缺索引