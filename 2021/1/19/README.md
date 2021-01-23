# Move fast and fix things

* Author: Vicent Martí
* Link: https://github.blog/2015-12-15-move-fast/

Github 详细的介绍了一次底层能力重构的过程，对于如此大规模的服务系统来说，替换 `git merge` 的实现是一件高风险操作，这篇文章展示了 Github 是如何进行测试、小流量启动的过程，非常有价值的部分是 Github 分享了在线上小流量过程中发现的 Bug 列表（包括若干个高危 Bug！）。
