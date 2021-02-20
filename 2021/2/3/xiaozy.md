<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [M3: Uber’s Open Source, Large-scale Metrics Platform for Prometheu](#m3-ubers-open-source-large-scale-metrics-platform-for-prometheu)
  - [M3 是什么](#m3-%E6%98%AF%E4%BB%80%E4%B9%88)
  - [为什么需要 M3](#%E4%B8%BA%E4%BB%80%E4%B9%88%E9%9C%80%E8%A6%81-m3)
    - [诉求](#%E8%AF%89%E6%B1%82)
  - [M3 简介](#m3-%E7%AE%80%E4%BB%8B)
    - [规模](#%E8%A7%84%E6%A8%A1)
    - [特性](#%E7%89%B9%E6%80%A7)
  - [设计目标](#%E8%AE%BE%E8%AE%A1%E7%9B%AE%E6%A0%87)
  - [设计方案](#%E8%AE%BE%E8%AE%A1%E6%96%B9%E6%A1%88)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# M3: Uber’s Open Source, Large-scale Metrics Platform for Prometheu

> 原文地址 https://eng.uber.com/m3/

## M3 是什么

M3 是 Uber 的监控平台

## 为什么需要 M3

没有 M3 之前，Uber 监控系统是 Graphite（采集） + Carbon（存储） + Grafana（看板） + Nagios（报警），存在几个问题

1. Carbon 集群扩容需要手动的 resharding
2. 缺乏数据冗余，任何一个节点的 node 故障，都会导致丢数据

### 诉求

1. 提升可靠性和扩展性：能够保证在不丧失可用性、准确性的情况下进行持续扩容
2. 能够跨数据中心查询全局数据，提供全局视野
3. 低延迟：保证看板和报警的低延迟
4. 灵活的 tag 能力
5. 良好的兼容性，兼容已有的 StatsD 和 Graphite 指标

## M3 简介

### 规模

1. 总 TS 数量 6.6 Billion
2. 采集速率 500M/s
3. TS 写入持久化内存 2M/s
4. 副本数量 3

### 特性

1. 允许指定不同粒度（1m、1h）的 metric 存储时间（rentention）

## 设计目标

1. 优化 metircs pipeline， giving engineers as much storage as possible for the least amount of hardware spend.

2. 数据高度压缩，基于 Gorillia's TSZ 压缩，优化而来

3. 降低内存使用，避免内存称为瓶颈，每个 shard 的 timewindow 内维护一个 Bloom Filter，以及一个 index summary

4. 避免 Compaction，引入 Downsampling，从而提升机器的资源利用率，保证读写延迟稳定

## 设计方案

1. Remote Region：类似 thanos，M3 Coordinator 可以从本 region 的 M3DB Cluster 读数据，也可以从其他 region 的 M3 Coordinator 读数据，顶层的 M3 Coordinator Fanout 到它能访问的所有 Remote Region，查询会优先下推到 Remote Region 计算

2. Fewer Compactions：Cassandra 等其他系统浪费了很多资源在 Compaction 上，而这些操作只写重复数据，M3 只做必要的 Compaction，比如 Backfilling 或者合并两个时间窗口的索引

3. Downsampling：在数据采集的时候做 Downsampling，从而避免多次 Compaction，以达到节省存储空间和计算资源的目的，支持流式和批量两种 Downsampling 方式

4. Resolution & Rentention：对于不同的 Rentention 采取不同的 DownSammpling 策略，例如 2d 的数据不做 Downsampling，30d 的数据 Downsampling 到 1m

5. Rollup：类似 Prometheus，在采集时做预聚合，目前还不能跨 Prometheus Instance 做 Rollup，只支持在单个 Prometheus Instance 上做 Rollup
