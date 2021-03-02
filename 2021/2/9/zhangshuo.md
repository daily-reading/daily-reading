原文：https://engineering.fb.com/2021/02/09/developer-tools/minesweeper/

### Minesweeper, 自动根据症状识别 bug 的原因

#### 如何工作
1. 捕获遇到 bug 前的操作，按时间顺序排序【trace】
2. 将问题 trace T 与正常 race C 比较
    1) 提取两者的 pattern (事件发生的时间顺序)【可以简单理解为接口调用顺序】
    2) 计算 pattern 的 precision(pattern 在测试组而不是对照组的占比) 和 recall(pattern 在测试组的占比)
    3) 计算 precision 和 recall 的谐波均值
3. 按照谐波均值排序，越高的，有问题的可能越大

#### 大规模自动 RCA
1. 使用 PrefixSpan 算法，从成千上万条 trace 中找出有效 Pattern

感觉可以和报警联系起来，捕获报警前一段时间的 trace，分析拿到谐波均值列表，在报警信息上展示

