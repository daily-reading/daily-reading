# Using K8s Admission Controllers to Detect Container Drift at Runtime

* Author: Sai Diliyaer
* Link: https://medium.com/box-tech-blog/using-k8s-admission-controllers-to-detect-container-drift-at-runtime-cc0f6c67c583?source=rss----5968e5293763---4

Box 遇到的问题是尽管集群上大部分的容器都是用 gitops 来管理的，但 kubectl 还是有能力直接去做状态变更。所以 Box 用 Admission Controller 实现了对这类 pod 的清理能力。
