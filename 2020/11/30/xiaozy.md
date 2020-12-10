# Introducing Dispatch
> 原文地址 https://netflixtechblog.com/introducing-dispatch-da4b8a2a8072

- 目标：
    - 重用现有熟悉的工具，降低学习曲线
    - 分类、存储、分析故障数据，帮助加速解决故障
- 工作流
    - 自动或用户手动创建 Incident -> Dispatch API -> Dispactch DB/ Create Indicent Flow
    - 优点
        - incident commander 不需要维护故障相关的资源或数据流
        - 标准化的沟通方式
        - 自动根据故障的类型、优先级等信息找到 participants
        - 追踪 incident，并在没有按时完成时通知相关人员
        - 所有的故障数据都是中心划管理的，不需要在多个工具之间跳转
        - 公共 API
- 总结
    - Dispatch 工作流和常见的故障管理工作流基本一致，基本职责包括
        - 故障元数据管理
        - 故障处理人员管理（拉那些人来处理、如何通知）
        - 故障生命周期管理
        - 事后复盘，从故障中学习
