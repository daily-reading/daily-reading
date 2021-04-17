1. availability and scalability come first. NO PAUSE TIME.

2. Use existing tools if it meets need. if need more, build in-house solutions.

3. Storage: Schemaless, Riak, Cassandra. Use different storage for different purpose (perf, legacy data, etc.)

4. Redis, Tweemproxy, Celery workers for caching.

5. Kafka + Hadoop for logging

6. Docker for app provisioning. Aurora for long running service and cron jobs.

7. Haproxy and hypoerbahn for SOA. frontend server is using nginx. And changing from poll based features switch to pub-sub pattern.

8. Phabricator, OpenGrok, Github for development (Phabricator现在还好用吗...我记得几年前用的时候界面好丑...)
Jenkins for CI. Packer, Vagrant, Boto and Unison for building and management.

Note: 好多的tooling，integration会花不少时间吧...有没有一套系统提供所有的服务呢（我觉得我是被azure dev ops惯坏了...）

9. Go and Java. Rip out python while migrating to micro service.

10. Hailstorm and uDestroy for testing. 

11. Reliability: Nagios for monitoring. people get paged for code they wrote (everyone on call 24/7?挺好的，这样都不敢瞎写bug了)

12. Telemetry: M3. Anomaly detection: Argos, 用model来预测...确实高级点...我们是简单粗暴设threshold。。。

13. Data analysis: Storm, Spark, React

14. Map: Gurafu （链接有点意思，收藏下有空看看）。µETA for ETA.


MARKETPLACE: UBER的frontend?

15. NodeJS: 没有perf问题吗？

16. Challenge: realtime. Ringpop solves the problem

17. Cassandra and Go for speed and throughput

18. python with flask and uWSGI for optimization and balance

19. data analysis: D3, react, mapbox etc.

WEB and MOBILE (这个才是frontend吧...所以marketplace更像是一个middle tier?)

20. Express.js for web server

21. application layer uses React and Flux. Use gulp for build.

Note: 做架构师还是挺不容易的=。=