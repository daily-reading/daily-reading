## How LinkedIn scales compatibility testing
> 原文地址：<https://engineering.linkedin.com/blog/2020/compatibility-testing/>

### Introduction

Our source control branching model is **trunk-based development**, meaning that developers push code changes frequently to the main branch of a multiproduct and avoid long-lived feature branches. 

**trunk-based development**: 开发者直接将代码更改提交到main branch

A key feature of multiproducts that makes our development ecosystem truly scalable is that library producers and consumers can release software independently. 

**independent**: 通过将 producers 和 consumers 相互独立来保证开发生态的 scalable

The strategy that enables this autonomy is to follow the **semantic versioning (semver) contract**. Specifically, we leverage the builds and tests of consumers to validate backwards-compatibility of code submissions for producers. We call this check compatibility testing.

**compatibility testing**: 利用消费者的构建和测试来验证生产者提交代码的向后兼容性。

### Compatibility testing

Our CI system runs two main validation workflows for every code push to a multiproduct.

**pre-merge validation job**: 代码合并前的 lightweight checks

**post-merge validation job**: 构建多产品并在发布新版本之前的 more extensive checks

这部分大致流程是 build -> find consumers -> upload build artifacts -> Trigger compatibility testing job for all consumers and collect the results

### Developer productivity challenges at scale

开发人员大规模增长带来的挑战:
1.  a single code submission to gradle-jvm would take 14 hours to complete due to the CI system’s validation of compatibility testing.
2.  every time that compatibility testing failed, library producers had to dig through the logs of each failed consumer (multiproduct), which was time-consuming.
3.  a small percent of consumers failed not because of the code changes under test, but because of their own non-deterministic (flaky) tests, producing a false positive signal that blocked the producers from publishing a new version of their library.

### Debuggability
CI system 中为帮助开发者调试而构建的部分功能：

1. The ability to reliably determine if a failure was caused by the infrastructure or by a legitimate issue in the validation. For each failure, actionable and fine-grained errors are displayed on the UI with links to the relevant execution logs and helpful resources (e.g., documentation) that can assist users to better understand and fix the issue, or triage it to the right team as needed.
2. A comprehensive one-page failure report displaying a holistic view of compatibility testing results. For each consumer, the report shows the pass/fail result, a summary of the failure cause, and other relevant information that helps producers identify issues more quickly, without having to go through individual logs for failed consumers.
3. Infrastructure support to temporarily store the cloned workspace of failed consumers and make them available for developers to debug.
4. Guidelines and tooling to seamlessly reproduce a consumer failure locally so developers can use their favorite IDE to debug.

### Stability

有时候兼容性测试不通过不一定是 producers 的问题，而是由于 consumers 的 non-deterministic (flaky) tests，而且在 producers 忙于 debug 的时候 consumers 有可能提交新的版本，导致追踪不到问题。

we built infrastructure support that empowers producers to make their code submissions resilient to consumer failures, while still receiving a reliable signal from compatibility testing.

为 library producer 提供了相应的配置参数：
1. Failure threshold. An upper bound on the percent of consumers that are allowed to fail without blocking publication of a new version (by default, zero
2. Ignored consumers. A set of known unstable consumers that may be ignored for each code submission irrespective of the Failure threshold (by default, an empty set).
3. Required consumers. A set of key reliable consumers with stable builds and tests that must pass against each code submission irrespective of the Failure threshold (by default, an empty set).

当然也有些紧急情况需要冒一定风险绕过兼容性测试:
Such code submissions will be marked with an override stamp and audited.

### Performance
We used software profiling to instrument our CI system’s post-merge validation implementation, and identified three main performance bottlenecks.
找出性能瓶颈进行优化，将时间从14小时优化至2小时

#### Job scheduler
we observed that oftentimes, a handful of long running jobs in each batch would create a bottleneck in performance for the scheduler, preventing it from kicking off more jobs.

最初的实现方式是每次提交一个batch，待batch中所有作业运行完再开始新的batch，因而少量长时间运行的作业会给调度器造成性能瓶颈

We enhanced the scheduler to trigger a new job as soon as a running job completes, so that at any given point in time, we maximize our resource utilization.

增强了调度器，使之在运行的作业完成后立即触发新的作业，以便在任何给定的时间点都能最大化资源利用率。

#### Compatibility testing preparation

Our original implementation used to query numerous backend services for each consumer, and download raw data over the network as part of this preparation, which was inefficient.

最初的实现为每个使用者查询大量后端服务，并通过网络下载原始数据，这是低效的

To optimize, we moved this preparation to the server-side and replaced expensive network calls with performant DB queries. 

将这些 preparation 转移到服务器端，并使用 performant DB 查询替换昂贵的网络调用。

#### Timely enforcement of the compatibility testing success criteria

We optimized the post-merge job to publish a new version of gradle-jvm as soon as a sufficient number of consumers have passed their validation such that even if all the remaining consumers fail, the total failures would still remain below the failure threshold.

对合并后作业进行了优化，只要有足够多的使用者通过了验证，就会发布一个新的版本

### Future work
1. Finer-grained error categories and UI enhancements
2. Auto-detecting flaky tests
3. Consumer prioritization