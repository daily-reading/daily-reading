这篇文章好应景，刚听完DDD的分享
先复习 一下Solid Principles
* Single-responsibility Principles 单一职责
    * Gather together the things that change for the same reasons. Separate things that change for different reasons.
    * 每个类有且仅有一个单一的职责。
    * 不仅仅是类，函数、方法也应该遵从
    * 任何 修改类/方法/函数 的行为，动机也应该是单一的
    * 在微服务中，
* Open-closed Principles 开闭原则
    * A Module should be open for extension but closed for modification.
    * 实体，应该对扩展开放，对修改关闭
    * 修改往往会带来隐藏的问题
    * 尽量去做扩展，不要去做修改
* Liskov substitution Principles 里式替换
    * A program that uses an interface must not be confused by an implementation of that interface.
    * 所有基类用到的地方，都可以替换成子类而不受影响
    * 继承是父类对子类的入侵
    * 一个接口的实现，是此接口的子类，此原则关乎接口或者抽象类的定义清晰
* Interface segregation Principles 接口隔离
    * Keep interfaces small so that users don’t end up depending on things they don’t need.
    * 多个特殊用处的接口好于一个“通用”接口 （看着代码令人惭愧）
    * 单一职责和接口 隔离的区别？
        * 单一职责注重 “高内聚”
        * 接口隔离注重 “低耦合”
    * 不仅仅是接口，而对于依赖，不去依赖不需要依赖的任何东西
* Dependency Inversion Principles 依赖倒置
    * Depend in the direction of abstraction. High level modules should not depend upon low level details.
    * 上层接口不依赖底层模块细节，只依赖于抽象
    * 抽象不依赖与细节，细节依赖抽象
    * 好的抽象屏蔽细节，拒绝网状依赖
* 补充一个 Law of Demeter 迪米特法则
    * 对于无需直接通信的实体，则不发生直接调用
    * 降低耦合度

