# How We Build a Design System

* Author: Jonathan Saring
* Link: https://blog.bitsrc.io/how-we-build-our-design-system-15713a1f1833?source=rss----5c2fdf847f4a---4

如何构建一套设计系统。不仅仅介绍了设计系统是什么，还介绍了怎么运营这样一套设计系统。

1. Visual Language
* Audit, then order: 
start from scratch当然更整洁，也更容易挣credit，但对于一个正在不断更新的系统，无法做到full stop and turn around。所以BIT所采取的策略是直接在现有的系统上AUDIT，整理，规范。这个需要有一个专门的组来进行，并且在进行中会涉及到很多组与组之间的交流。但好处就是，可以重用现有的系统，防止redundant，对已经使用了现有系统的用户，可以minimize影响。这里还提到了Visual consistency。对于任何一个UI LIBRARY，这个是基础，无法想象office fluent system或者Angular material中对于同一COMPONET的样式都不同一会是什么样子。同时，统一的规范也有利于A11Y的实现和enforcement.对于two assets，我的理解是，style guide是规范了UI在不同的scenario里长啥样，第二个是UX FLOW长啥样，并且在整个UX中， UI ELEMENT必须符合style guide。

2. Shared Components
pro: 各组独立开发，有针对性，开发进度会很快。
con: 容易创建出过于specific的UI element。例如team A创建了一个他们适用的list component，同时team B也需要一个list,但是TEAM A的LIST没有TEAM B需要的功能。这个时候，通常会有以下几种情况
    1.TEAM A把LIST refactor，作为一个新的shared list component，team B在不破坏现有功能的情况下，加入需要的功能。
        pro: reusable ui component.
        con: list component will become super large later on.
    2. TEAM B自己写一个LIST，放在自己的LIBRARY里。
        pro: no conflicts, earn credit.
        con: redundant code. Need to maintain 2 copies of code.
同时，shared component可能还会在integrate的时候，出现意想不到的bug。比较经典的例子就是Angular Material table里如果使用virtualization的话，sticky header就会break。之前联系过他们，他们的回复是it's on our backlog =.=

标准化开发的好处就是能尽量减少BUG。当然，还是需要code review把关。

3. Documentation and Discovery
通常不是DEV的业务，当然自己代码里的注释一定要写好，js doc用起来。

4. Incremental Upgrades
versioning。问题，需要做到backward compatibility。

5. Rippling Changes to Dependencies
我最近也在找一些找code dependency的tool，正好可以看一下。

6. Project Updates
nothing fancy.

7. Team Communication
nothing fancy，CI的一部分automation就是了。

8. Designer — Developer Collaboration
nothing fancy, 日常的吵架。

题外话：
如何解决highly customized UI component? 我还是比较倾向于Angular material的做法，对于一般的用户，使用Angular Material就可以，需要customized的，直接使用Angular CDK里的Library，自己拼装，customize，达到自己的需求。