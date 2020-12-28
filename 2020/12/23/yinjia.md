# How to Evaluate NPM Packages

> Link: https://thecarrots.io/blog/how-to-evaluate-npm-packages

作者建议比较当下流行的 React 组件库有几个量化指标，不要太过追究 APIs 和 UI 层面（虽然这些也是选择一个 package 的主要因素）

- sustainability 可持续性
- performance 性能
- security 安全性

1. Longevity

- Stars
- Used by
- Contributors
- Corporate Sponsor
- Age（[Tim Qian](https://github.com/timqian) 开发的评估工具[Star history](https://star-history.t9t.io/)）

2. Performance
   cost 查询工具（[bundlephobia](https://bundlephobia.com/)）

- Minified Size
- Tree Shakable
- Side Effects
- Dependencies

3. Security
   如果一个 package 的依赖越少，那它的风险也越小。可以用 `yarn audit`检查 package 的安全漏洞并推荐相关的补丁。
