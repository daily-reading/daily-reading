# [How to Evaluate NPM Packages](https://thecarrots.io/blog/how-to-evaluate-npm-packages)

作者介绍了一些自己选择第三方库时的考虑因素和建议

好的选择会让长期体验更佳而不是更痛苦（ If software has eaten the world, then open-source software propels it forward ）

- 光看 Star 是片面的，还要看如 Contributor 数量、Userd 项目数量、是否有 Sponsor 等
    - 作者给了一个查询 Star 历史的工具，输入仓库地址即可。如 https://star-history.t9t.io/#NG-ZORRO/ng-zorro-antd
    - 还要看下 Contributors 的活跃程度与分布，比如「 换届了 」或总是少数几个人维护就不太理想
- 性能上看最大的因素是包体积，以及「 弹性 」？—— 比如提到了 AntD 的一些 side effect（ 部分因为 momentjs 造成 ）使得模块的 tree-shaking 效果不佳
- 安全性基本能靠广大的 Contributors 和依赖方得到保证

除此之外想做两点补充：

- 文档。个人感觉从文档上也能看出一个团队在维护一个开源成果上的态度和能力。有些库的文档不好读不好找，有些文档缺失关键信息，这都是一些不友好或者可预见的隐患
- Issue。Issue 绝对数量多少是一方面，最好还要看下 Issue 上的更新频率，尤其是 Contributors 对 Issues 的关注度。如果大多数问题总是很久才解决，可能也是个隐患。以及从一些 Closed Issue 的原因上能看出些许开发团队的立场和方向是否与自己团队的一致或相符，比如我们很重视 TS 的严格性，但因此提出的议题别人却觉得不重要因此被忽略，可能就会出现分歧甚至之后更大的问题
