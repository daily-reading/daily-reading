## State machines are wonderful tools
> 原文地址：<https://nullprogram.com/blog/2020/12/31/>

状态机有诸多优点，如：
1. 简洁的接口
2. 少量和固定的资源
3. 无需关注输入输出
4. 实现简单
5. 易于理解

举了一些例子：
- 用状态机实现摩尔斯码解码器：摩尔斯码可以由一个字典树表示，但在这里我们考虑用状态机实现解码。具体来说我们将字典树的每一个节点都存入一个table中，再定义相应的状态转换关系。
- utf-8 解码器：
与摩尔斯码解码器不同，用一个有向图来表示utf-8的状态转移关系，转化成的状态表相比摩尔斯码更大，更复杂。Like Bjoern’s decoder, there’s a code point accumulator. The real state machine has 1,109,950 terminal states, and many more edges and nodes.
- 单词计数：相对简单一些，没有使用查找表，类似边缘触发机制，通过输入是否是空格来在两个状态下转换，通过正负来区分状态。
- 使用协程实现状态机，用yield特性实现状态转移操作

总的来说，状态机解法大致包括：建立状态转移关系图->建立查找表(状态转移规则)->运行状态机 三个步骤