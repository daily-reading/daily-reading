# Our evolution towards T-REX: The prehistory of experimentation infrastructure at LinkedIn

* Author: Alexander Ivaniuk
* Link: https://engineering.linkedin.com/blog/2020/our-evolution-towards-t-rex--the-prehistory-of-experimentation-i

这篇文章简单介绍了 LinkedIn 的 A/B 测试平台的演进历史，但感觉干货不多，没有包含多少工程实现细节。

## A/B 测试平台组成结构

- 目标定位系统 (targeting)
- 动态配置系统
- 试验的基础设施
- 分析和报告的 pipelines
- 通知系统
- UI 界面

## 什么是 A/B 测试

A/B 测试是开展研究的一种科学方法，包含对照组和实验组。它依赖于将测试人群随机分成多组，并向他们提供一些区别对待的策略。
在这种实验中，需要保留一个原始样本群体作为对照组，用作衡量其他实验组效果的基线。

![AB测试介绍](https://content.linkedin.com/content/dam/engineering/site-assets/images/blog/posts/2020/09/trexintro2.png)

在线服务中，常用 A/B 测试来衡量新功能对服务用户的影响。


为了开发A/B测试平台，必须从定义系统的四大能力入手:

- 评估一个用户是否属于或有资格进行A/B测试。
- 将一个用户分配到特定的测试组中。
- 将测试及其定义的信息传播到生产环境中，并记录用户在测试组中的信息
- 计算A/B报告并衡量测试的影响

## LinkedIn 的 A/B测试平台演进史

第一版LinkedIn实验平台非常简单，功能如下:

- 每个用户都被分配了一个独特的标识符，也就是 "member id"
- 整个会员群体被分成1000个 bucket，0-999，通过一个简单的除余公式，即bucket_id = member_id % 1000，将一个会员分配到一个桶里
- 为了运行一个测试，开发人员必须为其分配一个bucket的范围，并确保范围内的每个bucket没有被其他测试使用
- 测试的 bucket 范围被划分为对应测试集的子集

在早期间的 LinkedIn 的系统里，这套方式还比较简陋:

- 首先，将成员分配到A/B测试的过程并没有遵循随机对照试验的科学方法，因此可能会产生无效的结果。
- 第二, 没有用数据库来管理测试集的分配
- 第三，代码和A/B测试定义耦合严重，新增一个测试定义就要多加一段代码
- 第四，如果将 A/B 测试推上线，相应的业务服务也要进行重新配置并且部署上线, 耦合严重
- 第五, 没有自动化报表结果生成系统，需要手动写脚本分析

![test-arch](https://content.linkedin.com/content/dam/engineering/site-assets/images/blog/posts/2020/09/img-3-abtest.png)

后来 LinkedIn 因为不断增长的 A/B 测试需求，对这套系统进行了重构，结构如下:

![test-arch1](https://content.linkedin.com/content/dam/engineering/site-assets/images/blog/posts/2020/09/trexintro7.png)

新架构的优点之处在于:

- UI 产品化，提升了易用性
- AB测试实验的部署延迟降低到5min以内
- 拥有自动化的实验目标定位系统(Targeting)，和业务充分解耦
- 提供 DSL 作为主要自定义 Targeting 和实验的接口 (和系统解耦)


### 执行性能优化

LinkedIn 后来使用了多级缓存来优化 A/B 系统的查询延迟:

- 在 biz service 判断 A/B 测时, 首先先通过 A/B 测试的 SDK 来判断实验策略是否命中
- SDK 客户端尝试用已知的实验定义和实体属性的内存缓存在本地进行评估，这里的缓存命中率为98-99%
- 如果本地缓存 Miss 了，就会向 A/B Test 后端请求，获取实验结果
- 如果后端服务有缓存则返回，没有的话则从 KV 数据库中获取属性，在缓存以后进行评估并返回处理结果.

![teest-arch2](https://content.linkedin.com/content/dam/engineering/site-assets/images/blog/posts/2020/09/trexintro9.png)

得益于缓存系统，大概只有不到0.2%的评估请求会穿透缓存打到 KV 存储系统上。


