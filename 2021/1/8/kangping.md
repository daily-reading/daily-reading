# Building On-Call Culture at GitHub


* Author: Mary Moore-Simmons
* Link: https://github.blog/2021-01-06-building-on-call-culture-at-github/

和很多团队一样，Github 在人数膨胀、业务扩张之后遇到了 OnCall 工程师团队不足以支持整个服务的问题。这篇文章分享了他们是如何从文化层面去适应这一变化的。


## Monolithic On-Call
Github's codebase is monolithic, which is probably similar to a lot of places. The main pain points are therefore:

1. impossible for everyone to know everything, which means most of the time, on-call person just escalate. 
2. Rotation is huge, engineers were only on-call ~4times per year for 24hours. (??) `This seems insane to me.`
3. No systematic management of monitoring and documentation. Each person only have to do on-call sporatically, ad-hoc fixes are probably enough. (No reason to build a system!)
4. A small number of experts get alerted/ escalated to very frequently. 

## Solution
The solution is to split on-call rota to small teams. there are several techincalities to do this on a monolithic codebase, but more importantly, there are some cultural/educational hurdles.

### Logistical hurdles
1. File ownership. Map file/app to person/team and store the data systematically.
2. Monitoring/alterting. alert the appropriate team

### Cultural and educational hurdles
1. Training/Communication with engineers/ especially those who never been on-call before.
2. Work-life balance. Quick two-hour sub on-call or follow-the-sun different timezone on-call strategy. (This is extremely useful from my personally experience. Imagine sitting in NY and doing on-call for London stocks trading. Nightmare. )
3. Blameless culture. (I think different people probably reacts to "blame" differently, but certainly younger generation reacts more positively to more supportive culture.)
4. different level of cirticality of services/
5. dedicating time. recommend 20% of time spending on improving on-call experience.


