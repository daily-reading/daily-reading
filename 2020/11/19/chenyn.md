# [How We Build a Design System](https://blog.bitsrc.io/how-we-build-our-design-system-15713a1f1833)

—— Design System driven by Components

- 设计语言保证了设计一致性，设计一致性是实现组件的基石，因为 UI-UX 的可复用单元是更细粒度的，描述起来可能要很多文字和配图
- 组件是要被共享的，所以作者的团队选择了 Bit
    - Bit 基于 Base-UI 产生了 Evangelist，我理解为「 符合基础规范的定制化产物 」，还有模板、环境、流水线等机制保证组件产出过程中各个环节的尽可能一致
    - The design system’s team role is to facilitate and regulate, not block or enforce. 很赞成这句话
    - 作者认为这是很大的成功，因为可以独立开发但共享使用，大家可以减少各自的「 构造成本 」
    - 多平台框架无关也是个趋势。但..「 We’ve even been working with the Angular team itself to provide support for Bit with Angular. After taking everything into account, we believe React is the best solution for us at this point. 」😂
- 友好的文档 —— 可索引、好触达，对于共享的组件是很重要的
- 组件之间独立的版本控制会更好控制 —— 只在必要时升级，出问题时可以单独回滚或修复
- 因为组件可以相互依赖和组合，版本依赖关系的发现也会变得很有用
    - 如一个基础组件升级后，在一个项目中的所有组件如何依赖了不同版本的这个组件，至少会有提醒。Bit 甚至有能力集成 Github，来提前「 预警 」它们
    - 这个概念作者称之为 Ripple，感觉点像依赖解析，因为 Bit 能「 掌控 」其平台上的所有组件，所以能去「 连接 」它们做强版本控制
    - 版本变更的通知有利于团队的沟通
- 一个设计系统需要明确的职责划分
    - Designers usually talk about elements on their canvas.
    - Developers talk about the React component in their IDE.
    - Users will get the code, not the design.
    - 同时也需要良好的协作方式。作者提到了 Zeplin 与 Bit 的打通
- 最后，为什么我们需要设计系统保证一致性？Helping users intuitively navigate, and successfully interact, with different parts of your applications without confusion. **This is your brand. You need a good brand.** 品牌提升影响力
