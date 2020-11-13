这篇文章好应景，刚听完DDD的分享
先复习 一下Solid Principles
* Single-responsibility Principles 单一职责
    * 每个类有且仅有一个单一的职责。
    * 不仅仅是类，函数、方法也应该遵从
    * 任何 修改类/方法/函数 的行为，动机也应该是单一的
    * 在微服务中，
* Open-closed Principles 开闭原则
    * 实体，应该对扩展开放，对修改关闭
    * 修改往往会带来隐藏的问题
    * 尽量去做扩展，不要去做修改
* Liskov substitution Principles 里式替换
    * 所有基类用到的地方，都可以替换成子类而不受影响
    * 继承是父类对子类的入侵
    * 对继承做约束，这个约束就是本原则，不去列举了
* Interface segregation Principles 接口隔离
    * 多个特殊用处的接口好于一个“通用”接口 （看着代码令人惭愧）
    * 单一职责和接口 隔离的区别？
        * 单一职责注重 “高内聚”
        * 接口隔离注重 “低耦合”
* Dependency Inversion Principles 依赖倒置
    * 上层接口不依赖底层模块，只依赖与抽象
    * 抽象不依赖与细节，细节依赖与抽象
    * 好的抽象屏蔽细节，拒绝网状依赖
* 补充一个 Law of Demeter 迪米特法则
    * 对于无需直接通信的实体，则不发生直接调用
    * 降低耦合度


