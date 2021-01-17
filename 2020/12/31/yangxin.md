# Building On-Call Culture at GitHub
- 随着 github 的发展状态，作者团队需要不断地更新 on-call 策略，维护与研发人员之间的信任。
##  Monolithic On-Call 
- pain points 
  - 功能太多，oncall 的工程师没有信心能 handle 事情，最后会发展成只是一个接线员。
  - oncall 人员多，被轮到的时间不是很多，对工作不够熟悉，没有自信
  - 文档没有很好地维护
  - 大多数工程师对 oncall 没信心，最后只会导致最有经验的几个工程师需要出现在每一次 oncall 中。
  
## New On-Call Culture
- 划分 oncall 职责，对自己团队的代码 oncall
- 逻辑上的一些障碍
  - File Ownership
    - 文件映射到服务，明确 Ownership
  - Monitoring and Alerting
    - 对自身职责范围的 Monitoring
  - checkList 推动团队去做
- Cultural and Educational Hurdles
  - 适应新冠疫情带来的负面情绪影响，推动项目是更富有同理心，共情
  - 培训 oncall 人员，提供优质文档
  - 保证 oncall 的时候还能 WLK
  - Cultivating a blameless culture  create a sense of safety about making mistakes on the engineering teams
  - Meeting the criticality needs for each team
  - 花时间专注于长期的改善，比如文档的补充
