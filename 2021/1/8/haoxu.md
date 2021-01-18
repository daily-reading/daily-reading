下周要BACKUP ONCALL了，我也算ONCALL好几年了，结合文中提到的问题，正好说说我的感想
1. "The on-call rotation was large and engineers were on-call for 24 hours at a time"
一次ONCALL才24小时我觉得没有任何意义。通常都要一周左右吧，至少要让ONCALL的人能把身心都投入到ONCALL的节奏中去。
BTW，我们的oncall rule是，ON CALL的人不会被安排任何的feature work，2周的ONCALL (BACKUP&PRIMARY)专心处理customer reported incidents以及live site alerts.

2. "the monitoring and documentation was not well maintained"。这个我觉得是essential的，文档都不齐怎么干活。
我们有一个TEAM WIKI，基本上所有关于ALERT或者LIVE SITE上常用的Troubleshooting guide和sample query都会有。
每个feature的TSG都是在feature release前必须和CSS TEAM开会做交接。
所有feature的owning team都必须写清楚，这样在transfer incident或者involve another team时才会有依据。
每次oncall开始前和结束时，IM(incident manager)和上周AND本周的primary SME都会来一个短的standup，一方面将未处理完的incidents做一个交接，另一方面总结下出现的问题，以及如何improve。

顺便讲一下我们的oncall structure。
我们有一个live site的组挡在CSS和ENG TEAM之间，所有的alerts和incidents都会由他们先处理，他们无法处理的才会转给ENG TEAM。
作为ENG TEAM，最大的希望是2点
*misrouted incident
可惜我们组是saas组，基本上什么问题都会第一时间发给我们，这里有一个很有“意思”的情况，由于我们组处理incident很给力，并且有时候还会帮其他组也处理一些incident，这导致live site养成了一个坏习惯，每当他们不知道这个incident给谁的时候，都会给我们, SaaS team will figure out。这个问题直到现在都没能很好解决，我们的处理方法是在incident上mark一个poorly handled，然后再WSR上抱怨。对于不是我们OWN的INCIDENT，第一时间甩锅，不要犹豫。可能稍微有点用把。

*good quality incident，没有LOG，讲不清楚的incident（很多CSS都是外包给天竺国），很多次都会misunderstand customer的意思。导致INCIDENT都必须要打回给CSS，往返好几次才能彻底理解到底是什么问题。这个问题我们后来专门派人去CSS那边，和他们深入的交流了一下，把我们的workflow和他们的workflow都好好的沟通了一下，主要问题是要让CSS能够理解我们的PRODUCT和FEATURE，明白他们是如何supposed to work才行。现在的incident quality已经很好了。

3. Training touchpoints。新人6个月内我们不会安排进on call rotation，第一次oncall时候会有一个senior来shadow，全程从backup到primary带一次。

4. 文中file ownership那个我不太喜欢，如果是一个shared file across teams怎么办，我还是倾向于feature-wised ownership来区分responsibility。同时git也有commit history，谁regress的谁own。

5. “Monitoring and Alerting”，这个主要要处理的问题其实应该是false alarm和noise。可以通过调节threshold和让livesite干脏活来完成。

6. "Instilling work-life balance while on-call"
我一直信仰2点
* ONCALL就好好的ONCALL，be responsible
* ONCALL完如果被PAGE的次数比较多，MANAGER要适当的给点补偿，我听说GOOGLE是直接给钱。。。可惜我司实在太抠了。

7. some other thoughts
oncall的熟练度我觉得非常取决于experience。所以我们不会给进组6个月内的人安排ONCALL，直到他们熟悉我们的workflow, codebase, ES tools等之后。
ONCALL必须要做到**果断**，**脸皮厚**。我一开始时候太老好人，不是我们组的我也会去帮忙解决，只是因为我可以帮到他们。
如果是现在的我，肯定会第一时间把IM page进来，说明情况（不是我们的ownershio），invlove another SME and leave the bridge。该叫人就叫人，该甩锅就甩锅。即使叫错了，甩错了也不要紧，对内对外都可以说，我这么做都是为了尽快resolve incidents啊（战术后仰）。
BTW,我比较反感的是那种不做deep investigation，只是看到一点点和他们组无关的（比如stack trace上有一个call不是他们own的），就开始甩锅的人，通常我会直接把双方manager CC，让他们去撕逼。