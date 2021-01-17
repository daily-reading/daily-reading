# How we Build a Design System

> Link: https://blog.bitsrc.io/how-we-build-our-design-system-15713a1f1833

## Visual Language

> Audit, then order.

1. 对已有的样式和元素进行审查总结
2. 设计一致性系统及标准（颜色、字体、大小、位置、排版等）

设计系统的俩个基础部分：

1. 定义样式和实现的「 style-guide 」
2. 一组可重用的可视化元素，通过组件将 UI 和 UX 组合在一起

## Shared Component

在 [Bit](https://bit.dev/) 上可以共享组件生态，且各团队独立开发。

- 有 [base-ui Scope](https://bit.dev/bit/base-ui) 和 [Evangelist](https://bit.dev/bit/evangelist) (不是那么通用但是更具体化的组件)
- 主要以 React 为主（作者夸了下 React，特别是 React 16 出了 Hook 和 Context 后，各组件能够更好地解耦以及进行状态管理了）
- 标准化开发流程：大家可以共用一套模板、生产环境、流水线等。

## Documentation and Discovery

友好的文档管理

- 不需要为组件构建以及维护一个额外的文档点。
- 用同一套规范和设计，定制化和可重用性挺好的。
- 在本地可以看到 Bit.dev 的所有文档，版本更新友好。

## Incremental Upgrades

版本控制基于组件，每个组件都是一个 package，互不干扰。更容易升级、修复或者回滚。

## Rippling Changes to Dependencies

Bit 提供了 Ripple CI 管理各个组件之间的依赖。比如组件变更时，所有相关组件会重新构建并测试。

## Designer — Developer Collaboration

通过一种可视化的方式，Designer — Developer 可以更好地协作。

> Designers usually talk about elements on their canvas. Developers talk about the React component in their IDE. Users will get the code, not the design.
> （😺 没错）所以把设计师拉入我们的 UI 开发过程中来是很重要的。

- Zeplin: designers -> developers

- Bit: developers -> designers

  俩者结合，效率加倍。
