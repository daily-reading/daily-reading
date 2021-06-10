## Monorepo: please do!
>  原文地址：<https://medium.com/@adamhjk/monorepo-please-do-3657e08a4b70>

一篇反驳 Monorepos: Please don’t! 的文章

You should choose a monorepo because the default behavior it encourages in your teams is visibility and shared responsibility, especially as teams scale.
你应该选择monorepo，因为它在团队中鼓励的默认行为是可见性和分担责任，特别是在团队规模扩大的情况下

作者与Matt的共识:
monorepo必须解决polyrepo需要解决的每个问题，且存在鼓励紧密耦合，需要解决VCS可伸缩性问题的缺点

作者认为Matt的观点与很多开发组件的工程师相同:代码库笨重,难以测试等等;但作者认为他们忽视了一点:就是站在更高维度,不再以单一项目视角看问题时,会发现monorepo的价值: It forces the conversation, and makes trade-offs visible 它迫使团队之间进行交流,提高改动的可见性

假设有一个共享组件A, 团队BCD都在使用,团队A现在想改组件的API,如果是polyrepo,那么可能A就直接改了,不一定会通知BCD,BCD还在用之前的版本,等他们想升级的时候,才会发现api改了.这时BCD如果发现了种种问题,就不太好处理.但在monorepo中,这个改动必须和BCD交流,它使得改动无法绕开任何相关团队.

作者的总结:

In my experience, as the teams get big enough to no longer be able to hold the whole system in their head this is the most important thing in the world. You must raise the visibility of friction in the system. You have to work actively to force teams to look up from their component, and see the perspectives of other teams and consumers.根据我的经验，当团队变得足够大，不再能够把整个系统记在脑子里时，这是世界上最重要的事情。你必须提高系统中摩擦的可见性。你必须积极地工作，迫使团队从他们的组件上看问题，并从其他团队和消费者的角度看问题。

The default behavior of a polyrepo is isolation — that’s the whole point. The default behavior of a monorepo is shared responsibility and visibility — that’s the whole point


这个月来在推monorepo的过程中也有一些体会, Monorepo 可以提前发现 API 的不兼容性冲突, 也有利于基础组件的升级与推广.

例如在推的过程中发现,许多服务仍然在使用captain-client,commons-db 2.0等过时组件,这些服务在接入monorepo时,过时组件会出现不兼容问题,通过这个契机可以同时下掉/升级这些过时组件,包括后面我们计划要下zk,monorepo也算是一部分先期工作.

但这些不兼容问题同时也可能对升级带来风险,有些业务线版本迭代快,对接入monorepo非常支持和欢迎;但有些业务线可能5.6个人要负责200多个微服务,难以承受接入带来的时间成本和风险,如何尽可能的降低业务的接入成本和风险,是推广过程中需要解决的一个比较大的问题


