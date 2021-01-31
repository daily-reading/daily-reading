# How We Build Code at Netflix
[原文地址](https://netflixtechblog.com/how-we-build-code-at-netflix-c5d9bd727f15)

最近 coding 到上线总觉得不太舒服，瞄一眼 Netflix 的开发上线流水线是什么样的。

### 背景
对于采用的工具，团队有选择的自由但也同时背负维护的责任。服务治理上采用 AWS 方案，
微服务体系。

### Build
在 Gradle build 的基础上，提供了一套名为 Nebula 的插件，这些插件提供了额外的功能，
包括但不限于：
* Dependency management
* Release management
* packaging

### Integrate
由 Jenkins 接管，构建、跑测试和打包。
 
### Bake
部署时每次都新起一个 Amazon Machine Image，来从源头保证部署环境的一致性。
Bakery 暴露了一个创建此类 image 的 API，一些常规的约束、工具和服务在这一步被一起打包
到 image 中。

### Deploy
部署阶段，Spinnaker 有丰富的部署策略可选，比如金丝雀发布，红黑发布等策略。
回想自己现在部署时，stage之后流量马上全量放进来，没有流量分级接入，没有设施层面的快速回滚方案，
还是比较惨的。整个流程打包->stage->全量部署需要人肉盯着，也有点痛苦。