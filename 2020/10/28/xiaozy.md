1. yes-code software: 能够优雅的演进（gracefully evolve），应对需求和环境约束的变化
    > We need to be able to change software to accommodate changing circumstances without rewriting it, and that is fundamentally what software engineering is: how to change software systems. Change is the name of the game.
    1. 抽象：封装出独立(isolate)的部分，从而在不影响 code base 中其他部分的情况下进行变更
2. no-code software 无法应对变更和研发团队扩张上
    > Traditional software has learned the abstractions and patterns that make software resilient and adaptable to change and scale. No-code software is not ready for changing constraints nor development scale.
3. no-code software 适用场景
    1. Transitionary, ephemeral software：拥有预定义、有限的生命周期，不用担心变更和团队规模增长
        - 短期内不变，可以为求快而牺牲适应性
    2. High-churn code：代码频繁变化，但是不需要演进，今天写的代码明天就会被其他代码替代掉
4. 误区
    1. 软件工程不是寻求解决方案，而是如何演进方案，但是 no-code 工具并不关注这个
    > Software engineering is not about building solutions, it’s about evolving them. But change resiliency over time is not the focus of no-code tools
    2. no-code 工具是流程、工作流工具的扩展
    3. no-cdoe 工具并没有办法替代写代码
5. 个人总结
    - yes-code software 和 no-code software 是适应性和速度的权衡，需要决策者结合各自场景去判断
    - 没有银弹，不同的套路具有各自适用的场景
    - 想到之前看过的Thoughtworks CTO 对于 low code 的评论，大意是 low code 图灵不完备，复杂需求搞不定，堪称行业毒瘤，可以一起看看，[传送门](https://mp.weixin.qq.com/s/GwmHqBCoQzULba9UulqnPA)
