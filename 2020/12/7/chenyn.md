# [Why does it take so long to build software?](https://www.simplethread.com/why-does-it-take-so-long-to-build-software/)

- 一般工程的 complexity 分为 2 种
    - essential 来自被运用的知识或技能
    - accidental 来自工具或触达途径
- accidental 和 proficiency 有关，因此因人而异，也因此一个夹杂了 accidental 元素的 solution 会给带来很多不确定性
- 但是 accidental 的因素是无法避免的。You can’t solve a problem without some accidental complexity.
- 提到了 Conceptual Compression 这个名词
    - open source frameworks 很大程度降低了 accidental complexity。但面临其它问题，例如其中一点就是 ask more and more of our software，这使得工程变得不可能完全由写代码的人来控制，于是衍生出了很多东西
        - 多设备多端、拆分系统：如果设计的不够好，拆分会带来多余的交互复杂性，甚至不可预估，因为每个系统的走向已经不趋同了
        - 职责专一化：我理解是因为流程复杂所以拆分出来了多个团队，而且还在使用多种工具，这些都是 accidental complexity。如自动化基础设施及测试：无论是 Testing 还是 DevOps 的 CI/CD，都是为了在频繁迭代、部署的场景下维持系统的稳定性，就必须有专业的角色和手段来完成
    - volume of software within companies is exploding。企业内部的系统在爆炸式增加，但并没有加快每一个系统的开发效率。因为大多数系统之间会产生重叠，如果有交互就会产生复杂度，直到发现需要一种额外的「 能力 」能让各个系统自然集成。尽管这是一种技术生产力，但应意识到其带来的代价
        - [系统正在吞噬世界](https://a16z.com/2011/08/20/why-software-is-eating-the-world/)
    - new technology increasing。但不是技术越新越好，一是切换和普及有极高的成本，二是要看技术本身的发展阶段，例如有些成果没能跨过一些鸿沟就死了，这些是需要看到的，所以选型是非常有价值的技能
- 最后联想到几个实际问题
    - Angular 这种框架，给团队带来的 accidental complexity 就挺大的，很多时候业务团队因为要用 Angular 而花了大量精力在解决框架细节问题上，如果框架级别的功能出现问题还很难排查，这也是最近入职的同学会先抱怨 Angular 没有优秀的 DevTool 的原因，太浪费时间而看不到成效
    - 第二个是分支模型以及与 git 有关的 merge/rebase 问题，还有仓库的问题。本质上是因为流程带来了一定（ 可能还不小 ）的复杂度，一个团队需要为了维护这套流程及其配套的工具花费一定精力
    - 再有就是多端开发问题，这就是一个 Conceptual Compression？就像这篇 [The End of Cross-Platform as We Know It](https://medium.com/swlh/the-end-of-cross-platform-as-we-know-it-dad658d96b8)，为了跨端，所谓提高开发效率，才衍生出了框架工具，开始一轮一轮造轮子，这些都是 accidental complexity
