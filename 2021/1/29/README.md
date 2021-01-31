# Disaster Recovery for Multi-Region Kafka at Uber

* Author: Yupeng Fu and Mingmin Chen
* Link: https://eng.uber.com/kafka/

Uber 很可能运行了这个世界上最大规模的 Kafka 集群，这个集群也是 Uber 最核心的基础设施之一。在这样大的一个集群中，Uber 实现了跨 Region 的消息复制。本文分享了实现跨 Region 消息复制的一个关键算法。
