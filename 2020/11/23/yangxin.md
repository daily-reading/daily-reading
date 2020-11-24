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

## Developer productivity challenges at scale

  
