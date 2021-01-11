# State machines are wonderful tools

> 原文地址：Link: https://nullprogram.com/blog/2020/12/31/

1. 状态机有记下特点
   1. 足够小的接口
   2. 需要少量、固定的资源
   3. 不需要关注输入、输出
   4. 实现简单
   5. 容易理解
2. 给出了几个通过状态机的例子

   1. 摩尔斯解码器，根据当前的状态和读到的是 . 还是 - 来判断解码出的字符是什么
   2. utf8 decoder，类似 摩尔斯解码器
   3. Word Count，通过利用 "inside word" 和 "outside word" 两种状态直接的切换来实现流式的计数
      1. 初始状态为 outside word
      2. 状态转移规则
         1. outside -> inside: 遇到非空白字符
         2. inside -> outside: 遇到空白字符变
         3. outside -> outside: 遇到空白字符
         4. inside -> inside: 遇到非空白字符
      3. 每次 outside word -> inside word 时计数加 1

3. 介绍了如何通过携程来实现状态机，即通过 yield 来实现状态转移操作
