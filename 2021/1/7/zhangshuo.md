原文：https://blog.heroku.com/incident-response-at-heroku-2020

### Incident Response 流程

#### Page an Incident Commander
* IC 评估问题是否值得深入调查

#### Move to a dedicated chat room
* IC 创建聊天室，集中事件相关信息

#### Update public status site
* IC 指定 comms 及时更新事件状态
* 事件状态变动会通过多 channel 通知

#### Send  out internal Situation Report
* IC 向内部团队发送情况处理报告 sitrep
* sitrep 主要描述事件的现状和我们对它的响应

#### Access problem
* 更详细的评估事件
* 收集有利于问题解决的信息

#### Mitigate problem
* 对事件有了一定认识后，采取行动缓解问题

#### Coordinate response
* IC 负责选取对的人或团队处理事件

#### Manage ongoing response
* IC 负责通过 sitrep 及时同步事件进度

#### Post-incident cleanup
* IC 恢复事件处理过程中的操作

#### Post-incident follow-up
* 生产部门跟踪事件处理结果
* 每周业务回顾

### 角色
#### IC(Incident Commander)
#### COMMS(the communications role)

#### 思考
1. IC 和我们的 oncall 值班人员工作差不多，只是要投入更多到事件处理中，帮忙收集信息、评估等，而不只是分发
1. IC 很强调及时把事件处理进展同步到组内，oncall 是没有
2. 告警感觉也可以加入这个流程，每个告警都是一个事件，要有始有终

