# Daily Reading

每天一篇英文技术文章。希望通过这个阅读计划可以提升大家与英文阅读能力与技术视野。

## 规则

* 要参加 Daily Reading 计划的同学可以添加微信「e7hanz」加入微信群。
* 群里每天发一篇技术文章，文章链接也会提交到 Github Repo 上。
* 群里的同学每周至少提交一篇笔记，否则需要在下一周的周一离开这个群，离开的同学也欢迎随时回来。
* 笔记不限于当周的文章，仓库里的任意一篇文章都可以。

## 提交笔记

* 在相应的目录下创建个人名字命名的文件即可，例如「xiaoming.md」。

## Index

## Distributed System
* 2020/11/03 - [Cache is the Root of All Evil](2020/11/3): 深入浅出的讨论了最基本的缓存一致性问题，比如读覆盖、写后读问题等。
* 2020/11/01 - [Strategies Used at Box to Protect MySQL at Scale](2020/11/1): Box 关于数据库访问层 (Data Access Layer) 的一些实践。
* 2020/11/06 - [How We Learned to Stop Worrying and Read from Replicas](2020/11/6): 针对异步复制导致的主从不一致问题进行了讨论。
* 2020/11/09 - [Time, Clocks, and the Ordering of Events in a Distributed System](2020/11/9): 分布式领域必读 Paper。本文审视了在分布式系统中，一个事件发生在另一个事件之前的概念，并用它描述了事件的偏序关系。 给出了一种分布式算法，该算法用于同步逻辑时钟系统。
* 2020/11/14 - [Minimizing read-write MySQL downtime](2020/11/14): Yelp 的数据库高可用实践，分享怎么通过一个数据库代理中间层，实现多主切换的能力。
* 2020/11/18 - [The Google File System](2020/11/18): GFS，Google 的分布式文件存储系统。
* 2020/11/20 - [Cache coherency primer](2020/11/20): CPU 缓存一致性，详细的介绍了在多核、多缓存情况下如何确保 CPU 内的缓存一致性问题。和业务缓存的场景不一样，CPU Cache 是一个强一致性缓存。
* 2020/11/26 - [All things caching- use cases, benefits, strategies, choosing a caching technology, exploring some popular products](2020/11/26): 详细的介绍了缓存的适用场景、坑、收益，结合了从 CPU 到浏览器的各个层次缓存的实践分析。
* 2020/11/29 - [Orchestrating Cassandra on Kubernetes with Operators](2020/11/29): Yelp 将它们的 Cassandra 集群从虚拟机平台（EC2）上迁移到了 Kubernetes 中，本文分享了这一迁移过程的实践。
* 2020/12/01 - [Immutability Changes Everything](2020/12/1): 当计算和存储系统越来越便宜，并发锁同步成了系统瓶颈时，「不可变性」成了架构设计的一个趋势。本文讲述了在分布式系统设计里，「不可变性」是如何改变存储、计算和架构的。
* 2020/12/06 - [A Distributed Algorithm for Deadlock Detection and Resolution](2020/12/6): 一篇与死锁检测相关的论文，可以站在抽象的角度去思考，实际上描述的是分布式资源产生环状依赖关系的时候如何探测的问题，在分布式微服务的领域应该也可以运用到。
* 2020/12/08 - [Paxos Made Simple](2020/12/8): Lamport 在 2001 年重新写的一篇 Paxos 论文，用简单的语言介绍了共识算法 Paxos。Paxos 及各种变体广泛应用于各种分布式系统中。
* 2020/12/12 - [Scaling Cache Infrastructure at Pinterest](2020/12/12): Pinterest 的大规模分布式 Memcached 缓存实践。
* 2020/12/14 - [Scaling Memcache at Facebook](2020/12/14): Memcached 是一个非常简单的全内存分布式 KV 缓存系统，本文主要讨论了 Facebook 如何大规模使用分布式内存缓存来支撑其上繁重的网络请求。
* 2020/12/17 - [Structured Concurrency](2020/12/17): 本文作者 Martin Sústrik 在 2016 于本文首次提出了结构并发 (Structured Concurrency) 的概念，目前结构并发这一概念在各语言社区都被广泛讨论，有成为下一代编程语言新范式的潜力。OpenJDK 也有个进行中的项目 Loom，旨在 Java 17 中引入这个新的语言特性。本文可以说是结构并发的白皮书。清晰地阐述了这个新概念的起因与设想。
* 2020/12/31 - [Production Secret Management at Airbnb](2020/12/31): 随着团队的扩大，如何安全地保存项目中要用到的各种证书、密钥、密码成了一个挑战。Airbnb 通过一个 Bagpiper 项目来保存它们。这篇文章介绍了 Bagpiper 是如何做的。
* 2021/01/01 - [How Google Spanner Assigns Commit Timestamps - The Secret Sauce of Its Strong Consistency](2021/1/1): 介绍了 Google Spanner 作为一个全球分布式数据库，是如何使用 TrueTime API 解决全球强一致性问题的。

## Architecture
* 2020/10/30 - [Building Services at Airbnb, Part 4](2020/10/30): Airbnb 的微服务测试实践。
* 2020/11/07 - [Building Airbnb's Internationalization Platform](2020/11/7): 讨论了 Airbnb 的国际化平台架构，这是一篇业务架构综述文章，对 i18n 领域的常见问题进行了介绍，以及讲解了 Airbnb 的实现方案。
* 2020/11/08 - [Architecting for Reliable Scalability](2020/11/8): 文章讨论了在对架构进行扩展时的一些常见实践，例如模块化、水平扩展、内容缓存等等。内容中有大量的 Reference，是一篇很好的领域综述性文章。
* 2020/11/11 - [Taming Service-Oriented Architecture Using A Data-Oriented Service Mesh](2020/11/11): Airbnb 的 Mesh 实践。不同于现在到处都在说的数据面、控制面等概念，Airbnb 一开始就以数据为中心来构建整个服务治理系统。
* 2020/11/12 - [Solid Relevance](2020/11/12): 距离 SOLID 原则提出已经过了 30 多年了，这个时间太久以至于不少人认为 SOLID 原则已经不再适用于现代软件工程。但 Uncle Bob 认为并不是这样的，软件工程的根基一直没有变化，SOLID 原则依然适用。在本文中 Bob 重新审视了 SOLID 五条原则在现代软件工程中的适用性。
* 2020/11/17 - [Building Software Systems At Google and Lessons Learned](2020/11/17): Jeff Dean 在 2010 年的分享，分享了 Google 从 1999 年到 2010 年间面临的各种不同的挑战，以及架构的演进。
* 2020/11/24 - [On Designing and Deploying Internet-Scale Services](2020/11/24): 经典论文，讨论了设计一个后端系统时需要考虑的方方面面，例如面向故障设计、服务健康检查、底层组件零信任、版本管理、消除单点故障等等。架构师必读。
* 2020/12/04 - [How to Move Beyond a Monolithic Data Lake to a Distributed Data Mesh](2020/12/4): 文章列举了若干种不同的数据分析架构，着重对比了集中式的数据池（Data Lake）与分布式数据网格（Data Mesh）的差异，分析了之间的演进过程，提出了一些数据平台架构建设的建议。
* 2020/12/13 - [Choose Boring Technology](2020/12/13): 我们每天都要面对的问题是市面上可选的新技术越来越多，不管我们是否关心它们，新技术总会主动或被动的进入我们的视野中。如何看待新技术？如何在技术选型的时候平衡新技术带来的创造力和风险？本文的建议是，选择那些「无聊」的技术。
* 2020/12/19 - [How Netflix Scales its API with GraphQL Federation](2020/12/19): Netflix 是一个典型的微服务架构，微服务架构中，API 聚合层的复杂度会随着业务规模的扩展呈现出指数级增长。Netflix 通过 GraphQL 来解决这个问题，本系列文章介绍了 Netflix 的实践。
* 2020/12/28 - [Hints for Computer System Design](2020/12/28): 如何设计一个优秀的系统？作者从接口、实现等层次上分别讨论如何实现一个功能良好的、高性能的、高度容错的计算机系统。
* 2021/01/03 - [Data Mesh Principles and Logical Architecture](2021/1/3): 接 [How to Move Beyond a Monolithic Data Lake to a Distributed Data Mesh](2020/12/4) 一文，讨论了数据 Mesh 的架构与最佳实践。

## Network
* 2020/10/29 - [A Trip Down the DNS Rabbit Hole: Understanding the Role of Kubernetes, Golang, libc, systemd](2020/10/29): 一次 Kubernetes 中 DNS 的故障排查过程。
* 2020/11/05 - [Keeping Netflix Reliable Using Prioritized Load Shedding](2020/11/5): 当请求变多，流量开始变得拥挤的时候，Netflix 通过给流量标记优先级实现了渐进式地流量降级。文章详细地讨论了该系统的设计、困难与实际效果。
* 2020/11/13 - [How Facebook is bringing QUIC to billions](2020/11/13): Facebook 的 QUIC 实践，包括 QUIC 的收益、推广过程、评估体系以及一些坑。

## Storage
* 2020/12/11 - [Modern storage is plenty fast. It is the APIs that are bad.](2020/12/11): 现代 NVMe 设备改变了 I/O 操作的行为，让随机读的成本不再昂贵。作者基于 Intel Optane 设备设计了两个新的读文件 API，并且得到了不错的性能提升。本文讨论了如何设计 API 以适应更快的新型读写设备。

## Engeerning
* 2020/11/16 - [Building Netflix's Distributed Tracing Infrastructure](2020/11/16): Netflix 的分布式链路追踪系统实践。
* 2020/11/27 - [Edgar: Solving Mysteries Faster with Observability](2020/11/27): Netflix 的 APM 系统 Edgar，融合 Tracing、 Logging、Events，100% 采样，良好的场景化设计。
* 2020/11/30 - [Introducing Dispatch](2020/11/30): Netflix 的故障管理系统实践，包括通知、故障跟踪、故障复盘等环节。
* 2021/01/06 - [StreamAlert: Real-time Data Analysis and Alerting](2021/1/6): Airbnb 的实时报警计算平台架构介绍。
* 2021/01/07 - [Incident Response at Heroku](2021/1/7): Heroku 的的故障管理实践，包括响应、评估、协调、事后、升级等各个方面。这篇文章也 Reference 了几个 Heroku 在用的故障管理工具。
* 2021/01/08 - [Building On-Call Culture at GitHub](2021/1/8): 和很多团队一样，Github 在人数膨胀、业务扩张之后遇到了 OnCall 工程师团队不足以支持整个服务的问题。这篇文章分享了他们是如何从文化层面去适应这一变化的。
* 2021/01/09 - [Lessons learned in incident management](2021/1/9): Dropbox 的故障管理实践，分享了 Dropbox 如何优化检测、诊断、恢复三个流程去实现 99.95% 的可用性。这篇文章比 Heroku 和 Airbnb 的分享要细致很多。
* 2021/01/11 - [Maximizing Developer Effectiveness](2021/1/11): 文章提出了一个优化研发团队生产效率的框架, 作者通过明确反馈回路、优化反馈循环，让研发团队的效率更高。

## Frontend
* 2020/10/28 - [When is no-code useful?](2020/10/28): 讨论了低代码平台存在的问题，以及作者认为低代码平台要解决的问题。文章中对于复杂度、软件工程的本质提出了一些看法。
* 2020/11/19 - [How We Build a Design System](2020/11/19): 如何构建一套设计系统。不仅仅介绍了设计系统是什么，还介绍了怎么运营这样一套设计系统。
* 2020/11/21 - [Micro Frontends Pattern Comparison](2020/11/21): 微前端框架是近几年比较热门的一个领域，本文讨论了几种常见的微前端框架范式。
* 2020/12/10 - [The End of Cross-Platform as We Know It](2020/12/10): 跨平台框架是几十年来新技术层出不穷的一个领域，例如最近几年非常热门的 React Native、Flutter 等。作者认为这些跨平台应用有着无法解决的固有缺陷，要么改进，要么死亡。
* 2020/12/15 - [Errors and Error Handling in JavaScript](2020/12/15): 介绍了 JavaScript 错误捕捉的基本概念和实践。
* 2020/12/23 - [How to Evaluate NPM Packages](2020/12/23): 分享了作者是如何选择开源组件的，其中通过点赞趋势、开发趋势、性能、安全等多个方向去综合评估的思路值得借鉴。
* 2020/12/25 - [How we built the GitHub globe](2020/12/25): GitHub globe 是前一阵子 Github 做的一个 Web 可视化报表，展示了全球开源社区的生产活动。这篇文章分享 Github globe 是如何被实现出来的。
* 2021/01/04 - [10 Engineering Challenges Due to the Nature of Mobile Applications](2021/1/4): 面向几百万活跃用户去构建一个客户端应用程序同样需要高可用和可扩展性，这篇文章收集了工程师在构建大规模 iOS / Android 原生应用时面临的挑战。

## Personal & Team Work
* 2020/10/31 - [Engineering Onboarding Processes at Medium](2020/10/31): 了解 Medium 如何帮助新工程师融入环境。
* 2020/11/02 - [What do we need to know to start making a difference?](2020/11/2): 讨论了关于学习和实践的关系，作者将知识分为两类，分别讨论了如何获取这两类知识。
* 2020/11/04 - [Things I Learned to Become a Senior Software Engineer](2020/11/4): 当度过工作的第一年，发现简单的任务已经不再是挑战之后，如何在成为高级工程师的路上更进一步？Neil Kakkar 给出了他的一些思考。
* 2020/11/10 - [Taking Ideas Seriously is Hard](2020/11/10): 讨论了「familiarity」与「taking sth seriously」的区别，比较有感触的是「复利」对于「学习」的影响。
* 2020/11/15 - [Making the most of your one-on-one with your manager or other leadership](2020/11/15): 一篇小短文，讲怎么提升 one-on-one 会议的效率。
* 2020/11/22 - [How do you write simple explanations without sounding condescending?](2020/11/22): 把复杂的事情讲简单是个强有力的能力，这篇文章给出了一些建议。
* 2020/11/25 - [Growth as a writer](2020/11/25): Linus Lee 作为一个非常高产的博主，提出了他认为的写作的三个阶段。
* 2020/11/28 - [Common Performance Review Biases: How to Spot and Counter Them](2020/11/28): 在绩效反馈中，我们并不总是可以收到清晰、公正的反馈，有些反馈会带有一些偏见。作者总结出了一些常见偏见模式，并且针对主管和成员都给出了很好的改正建议。
* 2020/12/02 - [Attention is your scarcest resource](2020/12/2): 一些关于个人时间管理的技巧。当一个人可以用 50% 的时间专注在同一个事上时，TA 就会成为一个团队中最耀眼的明星。
* 2020/12/09 - [It's Not Just Standing Up: Patterns for Daily Standup Meetings](2020/12/9): 运转良好的每日站会给团队增加了重大价值，但有些做法会在站会上浪费所有人的时间。这篇文章介绍了常见站会的做法和收益，告诉大家怎么去调整那些浪费时间的实践。
* 2020/12/16 - [You don't have to be busy to be prolific](2020/12/16): 关于个人时间管理的实践，作者认为通过「创造性惯性」以提升工作效率是实现高效产出的最佳实践，而不是通过堆积工作时间让自己高产。
* 2020/12/21 - [Creating a culture of Initiative - the importance of failure](2020/12/21): 在建设团队氛围时，大家都希望团队是一个积极主动的做事方式。这篇文章围绕着「失败」这一问题，讨论了如何建设主动性文化。
* 2020/12/22 - [To listen well, get curious](2020/12/22): 一篇关于「倾听」的的 soft skill blog。前置技能是[非暴力沟通](https://book.douban.com/subject/3533221/)，一套简单、有效的沟通技巧。
* 2020/12/26 - [Life is Short](2020/12/26): 生命短暂，如何去使用这些宝贵的时间让自己感觉好一些？
* 2020/12/27 - [Build tools around workflows, not workflows around tools](2020/12/27): 好的工具应该完全适应个人的工作流，而不是让个人的工作流来适应工具。所以作者的选择是 - 自己写一套。
* 2020/12/29 - [Philosophy - Google Documentation Style Guide](2020/12/28): Google 的文档哲学，写出简单、可读、实用文档的技巧。
* 2020/12/30 - [I've conducted over 600 technical interviews on interviewing.io. Here are 5 common problem areas I've see](2020/12/30): 作者分享了在过去 600 次面试过程中，总结出的候选人常见的 5 个问题。
* 2021/01/05 - [Becoming a magician](2021/1/5): 作者认为「魔术师」是实现了知识上的层级跨越的人。在普通人的心智模型中，是很难理解「魔术师」是怎么去做的，但一旦你成为了「魔术师」，一切就变得理所应当。本文给出了一些如何成为「魔术师」的建议，其中最关键的一点是，想象力。这是一片反常规的关于自我成长的文章。

## Testing
* 2020/11/23 - [How LinkedIn scales compatibility testing](2020/11/23): LinkIn 有 12,000 个代码仓库，这些仓库之间有复杂的依赖关系。如何确保代码更新时对上下游的兼容性是可靠的？LinkIn 在这篇文章中分享了一些实践。
* 2020/12/18 - [Our evolution towards T-REX: The prehistory of experimentation infrastructure at LinkedIn](2020/12/18): T-REX 是 LinkedIn 建设了十年之久的一套 A/B 测试平台，本文以及后续的文章描述了这十年来 T-REX 的演进历史。
* 2020/12/24 - [How do you test your tests?](2020/12/24): TDD 的一个困难是测试逻辑本身的恶化难以控制，这篇文章提出了一种思路，通过工程化的思路去对测试质量进行衡量，找到那些退化了的自动化测试用例进行维护。

## Operation System
* 2020/12/20 - [Understanding Linux CPU Load - when should you be worried?](2020/12/20): CPU Load 是一个既基础又容易被误解的知识点，这篇文章举了一个车过桥的例子，生动、简单的说明了 CPU Load 和 CPU 利用率之间的区别，以及对于不同程度的 CPU Load 我们应该关心什么。

## Others
* 2020/12/03 - [Introducing Pipelines to Airbnb's Deployment Proces](2020/12/3): Airbnb 的发布流水线实践。
* 2020/12/05 - [GitLab.com database incident](2020/12/5): Gitlab 的在 2017 年出现了一次大规模故障，导致了网站 6 小时的数据无法恢复。故障原因是系统管理员误操作，以及多项备份措施无法生效。这是本次故障的外部报告。
* 2020/12/07 - [Why does it take so long to build software?](2020/12/7): 软件复杂度入门，科普了本质复杂度与意外复杂度的区别，以及现代软件工程面临的主要挑战。
* 2021/01/02 - [State machines are wonderful tools](2021/1/2): 状态机是一个很强大的编程思想，作者用摩尔斯电码解析、UTF-8 解码、字数统计为例说明了如何用状态机写出清晰、易于扩展的代码。
