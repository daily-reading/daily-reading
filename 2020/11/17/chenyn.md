# [Write Shameless Garbage Code](https://levelup.gitconnected.com/write-shameless-garbage-code-ba6f79d46ed9)

- 什么是 Garbage Code
    - Overly bloated and simple code that does, however, function correctly
    - 绝不是 insufficient, buggy code
- 几种明显是垃圾代码的标准：
    - 很难理解它在写什么、很难 debug
    - 写死的硬编码
    - 太臃肿或琐碎
    - 跑起来性能堪忧
- 不是不可以写垃圾代码，但是
    - 必须要意识到它们的存在 —— 比较常见的做法是打个 FIXME，能让 reviewer 知道你并不是无意为之
    - 需要尽可能将其与干净的代码隔离开 —— 修正或删除的时候很简单，摘除的过程是无痛的
    - 最好给一个 DDL 去清除掉它们 —— 比如随手记一个重构的 Backlog
