# How LinkedIn scales compatibility testing
- trunk-based development
- library producers and consumers can release software independently
  -  semantic versioning
    - integrated semantic versioning as a required check in our CI pipeline
    - 想起之前airbnb也说了不仅要定义最佳实践，还需要实现一些自动化工具来确保能真正大规模使用

## Compatibility testing
-  two main validation workflows
  - pre-merge validation job （fail fast）
  - post-merge validation job （比较耗时的）
- Compatibility testing is a required check that runs in the post-merge validation job
- 这个测试的意思是比如fenbi-rpc升级版本，就会拉所有用了rpc的项目做一个集成测试吗？

## Developer productivity challenges at scale
- 随着规模的增加，Compatibility testing耗费的时间增加
- 如果失败了，排查也很困难。
- hence must enhance the debuggability, stability, and performance aspects of compatibility testing

### Debuggability
- 解释了一下debug的复杂性，很多时候失败可能是网络，硬件故障等非代码因素
  - actionable and fine-grained errors are displayed on the UI with links to the relevant execution logs and helpful resources 、
  - comprehensive one-page failure report （不太清楚第一条和第二条的具体区分）
  - support to temporarily store the cloned workspace of failed consumers 保留现场
  -  reproduce a consumer failure locally

### Stability
- own non-deterministic (flaky) tests. 应该不允许维护这种单测
-  unstable consumer的上线可能导致produce a false positive signal
  - Failure threshold.
  - Ignored consumers
  - Required consumers
 
### Performance
- Job scheduler  分批执行到有资源空闲就调度执行
- Compatibility testing preparation 没看懂
- Timely enforcement of the compatibility testing success criteria 达到及格线了就结束，而不是等待所有完成

## Future work
- Finer-grained error categories and UI enhancements.
- Auto-detecting flaky tests (单侧写的不好？)
-  Consumer prioritization. 
