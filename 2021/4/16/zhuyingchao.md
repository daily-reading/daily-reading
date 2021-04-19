# An incident response starter-pack: how do you handle production outages?

## *Incident Roles*

角色有助于区分职责，确保某人正确地关注事件的各个方面。一般我们需要关心两种角色类型。所有角色参考：[Roles](https://www.atlassian.com/incident-management/incident-response/roles-responsibilities)

- *Incident commander* 
- *Communications*： 

## 解决问题的步骤
### 找到「出血」的原因
- 确定哪些系统发生故障，然后通过依赖性分析确定是由于上游组件还是下游组件引起的
- 验证从第三方获取的信息
- 故障影响范围分析

### 开始「止血」
我们的目标应该是立刻解决问题然后再进行清理工作，因此，我们需要确定行动的优先级。
- 回滚到已知的没有问题的版本。即使可以很快修复bug并上线，也应该在回滚后再做此操作
- 采取措施保护好关键性系统
- 充分利用团队资源

