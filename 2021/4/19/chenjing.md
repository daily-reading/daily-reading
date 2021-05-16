## Layers in Software Architecture that Every Sofware Architect should Know
> 原文地址：<https://levelup.gitconnected.com/layers-in-software-architecture-that-every-sofware-architect-should-know-76b2452b9d9a>

Layers are representing the different levels and types of abstraction of the concerns which accompany software development.
软件分层代表了软件开发过程中不同层次和不同类型的关注点抽象

### 表示层
表示层包含了用户体验的代码和技术,包括像Angular或Blazor这样的web开发框架，像WPF或WinForms这样较少使用的桌面框架，以及像Xamarin这样的移动开发框架。MVVM或MVC等表示模式也是这一层的一部分。

一旦需要实现实体行为、命令、查询或与外部系统的某些通信，这就表明该代码不属于本层。更准确地说，属于业务层。

### 业务层
业务层是负责软件存在意义的填充代码。实现软件功能的算法属于业务层。

输入层是来自表示层的数据结构，另一端是来自数据访问层的数据结构。输出是表示层所期望的数据结构或数据访问层所期望的数据结构。

业务层的输出数据结构的形式应该尽可能地满足其他两个层。

### 数据访问层
数据访问层负责连接到持久性。最常见的持久性是关系数据库，因此数据访问层通常包含对象关系映射(ORM)框架，如Entity framework Core或Hibernate。
