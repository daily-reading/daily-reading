# Pinterest Flink Deployment Framework

## 目标
* *Flink* 标准化构建
* 部署以及操作历史记录
* 消除重复任务

## 实现

* Bazel ：构建系统
* Hermez：发布系统
* Job Submission Service：作业提交服务
* YARN cluster：YARN集群

![Figure 1. Deployment high level architecture](https://miro.medium.com/max/1400/1*2SFdTktvUwn55nWq0-GtnQ.png)

以下为翻译
---

## 背景

**Apache Flink** 是一个框架和分布式处理引擎，用于在无边界和有边界数据流上进行有状态的计算。它提供了 *exactly-once*、低延迟、高吞吐量和强大的计算模型。*Pinterest* 将 *Flink* 作为统一的流处理引擎。

## 要求 or 目标
### *Flink* 标准化构建
在 *Pinterest* ，我们使用 *Bazel* 作为构建系统。我们需要一个标准化的 *Bazel* 规则来构建所有 *Flink* 作业，而不去改变 *makefile*。构建完成后，应该自动将 *jar* 上传到远程存储，而不是要求用户将 *Flink jar* 复制到 *YARN* 集群。

### 部署以及操作历史记录
用户习惯于将 *Flink jar* 复制到 *YARN* 集群中，并手动运行命令。如果我们需要恢复失败的作业，我们很难追踪以前的执行历史记录。我们需要提供一套标准的Flink操作，例如 *launching*、*killing*、*triggering savepoint*，以及从最近的 *savepoint* 恢复作业。

### 消除重复任务
*Flink* 应用程序是作为服务部署的，因此，每个 *Flink* 应用程序一次应该运行一个实例。我们需要防止用户意外地为同一作业部署两次，这意味着两个实例都可能写入同一个 *Kafka* 主题。这意味着向 *Kafka* 进行双重写入，这可能影响下游的工作。

## 发布框架
以下为整体架构图：

* Bazel ：构建系统
* Hermez：发布系统
* Job Submission Service：作业提交服务
* YARN cluster：YARN集群

![Figure 1. Deployment high level architecture](https://miro.medium.com/max/1400/1*2SFdTktvUwn55nWq0-GtnQ.png)

### 创建 * Bazel* 构建文件
构建文件需要包含 **load(“flink_release”)**，同时用户也需要插入以下 *Bazel* 规则：
![flink_release](https://miro.medium.com/max/1400/1*TiXBR32eOAEWMHm0WGu4iQ.png)

### 定义 *Hermez* 发布文件
*Hermez* 是 *Pinterest* 持续部署系统。为了使用 *Hermez* 启动 *Flink* 作业，用户需要创建一个 *Hermez.yml* 文件。文件包含的信息包括要运行*Flink* 作业的 *YARN*集群、要使用的 *YARN* 参数、要使用的资源等等。对于 *Flink* 作业的每个实例，用户应设置一个单独的 *YAML* 文件。例如，如果用户在 *dev*、*staging* 和 *prod* 环境中运行他们的作业，他们将需要三个不同的 *YAML* 文件（每个环境一个）。

以下是 *yml* 文件的示例：
![yml example](https://miro.medium.com/max/1400/1*uKWkaRG2ZWViz8EsGqyesA.png)

### 自动构建 *Flink* 作业
以下数字是指图1中的步骤：Deployment high level architecture

每当用户获得对 *Git repo* 的更改时，就会触发 *Jenkins* 任务来构建*Flink job jar*（1）。*Jenkins* 任务将遵循构建文件中描述的 *flink_relase* 规则来构建 *Flink JAR*，并将其上传到 *S3 bucket*（3）。同时，它将把部署相关的 *Hermez YAML* 文件上传到 *Artifactory*（2）。*Hermez* 会监听 *Artifactory*，当它看到一个新的 *yml* 文件时，它将在 *UI* 上显示它，以允许用户使用该 *yml*（5）启动作业。 

### 启动 *Flink* 作业
当用户启动 *Flink* 作业时，*Hermez* 将 *yml* 文件转换为 *JSON* 并将其提交给 *Job Submission Service*（*JSS*）（6）。*JSS* 是一个由*Pinterest* 维护的服务，它能够调度和启动 *Flink* 作业到 *YARN* 集群。

*JSS* 检查请求并确保 *Flink JARs* 和 *Flink job state* 存在于 *S3* （7）中。如果一切正常，*JSS* 将首先启动 *shell runner* 作业，该作业将在 *YARN* 集群（8）上执行命令。*shell runner* 作业从 *S3* 下载*Flink* 作业的 *JAR*，然后使用 *JSS*（9）提供的配置，启动实际的*Flink* 作业。我们添加 *shell runner* 作业的原因是将 *JSS* 保持为一个薄层（*a thin layer*），而不需要处理不同的客户端计算引擎（Flink、Spark、MapReduce等），以及每个集群的不同配置。

### *JSS* 去重
当我们恢复 *Flink* 作业时，我们提供了几个选项，包括从最近的 *savepoint* 或 *checkpoint* 恢复、刷新状态（* fresh state,*），以及指定 * savepoint* 或 *checkpoint* 路径。作业去重功能确保一次只运行一个 *Flink* 作业实例。

作业去重的工作方式是，提交作业时，每个作业都有一个唯一的名称。如果已经有一个作业实例在运行，*JSS* 将触发一个 *safepoint* 并先停止它，然后提交新作业。如果停止请求因为 * savepoint* 失败而失败，那么提交的请求将失败，并且正在运行的实例仍在运行。如果有一个部署正在进行，则新作业提交将被拒绝。

### *Flink* 作业配置热修复（更新）
由于 *Flink* 配置与 *Flink* 作业二进制文件打包在一起，用户通常会将配置更改签入（*check in*） *Repo* 并重新打包。整个过程可能需要10多分钟。如果我们想在事件期间快速调整参数，这可能是一个问题。例如，当 *Flink* 作业由于缺乏资源而在生产中失败时，我们通常会经历整个构建过程，以推出资源配置更改。事件解决后，我们需要签入（*check in*）另一个更改以回滚这些配置。为了加快这个过程，我们在 *Hermez* 上提供了一个热修复功能，可以在不更改代码的情况下覆盖 *Flink* 作业配置。用户可以在部署期间调整 *Flink* 配置值。在背后，*Hermez* 将直接覆盖 *Hermez* 从 *Artifactory* 读取的 *ymls* 中的这些值。

## 下一步是什么
### 减少部署延迟
当前的方法是先启动 *shell runner*。然后，*shell runner* 启动 *Flink* 作业到 *YARN* 集群，这可能会增加延迟。我们计划改进此过程以减少端到端*Flink* 作业启动时间。

### 自动作业故障转移
为了进一步提高平台和 *Flink* 应用的可用性，我们在多个 *AWS* 可用区域（AZ）中构建了 *YARN* 集群，以便在一个集群或一个AZ不可用时提供备份。我们还正在构建一个服务，它可以自动检测任何集群故障和故障转移失败的作业，以备份不同的 *AZs* 中的集群，或检测应用程序故障并自动重新启动应用程序。
