# Incident Response at Heroku
> 原文地址：https://blog.heroku.com/incident-response-at-heroku-2020:w

1. 处理故障时，除了技术性的处理问题，还需要协作和沟通
2. 故障处理流程
    1. Page an Incidant Commander: 确定故障的主 R
    2. Move to a delicated chat room：把相关方拉到一个 IM 群里
    3. Update the public site status：更新全站状态，每次状态更新都会告知用户故障的状态
    4. Send out internal Situation Report：向内部团队发送故障描述，好的故障报告应该帮助新的 Responser 快速了解故障现状，给与其他关注者上下文信息，应该包含以下内容
        1. （故障的）已知信息
        2. 跟进人员及职责
        3. 相关 issue
    5. Assess Problem：预估故障，故障大概的影响、可能的原因、严重程度
    6. Mitigate Problem：降级，减轻故障的影响
    7. Coordinate Response：协调响应，IC（故障主R）确保找到了应该处理故障的人
    8. Manage ongoing reponse: 同步故障状态（哪些解决、哪些正在处理），直到所有的问题解决完
    9. Post-incident cleanup: 清理故障处理过程中临时的资源、改动
    10. Post-incident follow-up：故障复盘 -> 可以有哪些改进
3. 当故障严重时，需要高管参与，高管不应该从外部信息源了解故障
