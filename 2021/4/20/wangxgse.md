# 提交说明
- 编号：NO.22@20210523
- 文章：[Effective Microservices: 10 Best Practices](https://towardsdatascience.com/effective-microservices-10-best-practices-c6e4ba0c6ee2)

# 读书笔记

微服务也是2012年之后应运而生
> One question may arise: why we need a new Software Development methodology suddenly? The short answer is that the whole ecosystem related to Software Development has changed significantly in the last decade. Nowadays, Software is developed using Agile methodology, deployed on Container+Cloud using CI/CD, persisted on NoSQL databases, rendered on a modern browser or smartphone and the machines are connected via a high-speed network. As a result of these factors, Microservice Architecture is born in 2012.

1. Domain Driven Design
    - If Microservices are not split in the right way, there will be tightly coupled Microservices which will have all the disadvantages of a Monolith and all the complexities of Microservices aka Distributed Monolith.
    - 微服务要正确拆分，否则会成为分布式单体应用
2. Database per Microservice
    - double edge sword    
3. Micro Frontends
    - Very often in Microservice projects, backends are very finely modularized with their database but there is one Monolith Frontend. 
    - Frontend Monolith is as bad as Backend Monolith
    - 这个观点对于我来说是新颖的，前端同样需要微服务划分，否则也会如服务端一样面临大单机带来的问题
4. Continuous Delivery
    - Using Microservice Architecture without CI/CD, DevOps, Automation is like buying the latest Porsche and then drive it with hand-brake. 
    - CI/CD is listed as one of the three prerequisites to use Microservice Architecture by Microservice Expert Martin Fowler.(https://martinfowler.com/bliki/MicroservicePrerequisites.html)
    - 要有分布式，必须要有Continuous Delivery
5. Observability
    - ELK/Splunk offers Logging for Microservices. Prometheus/App Dynamics offers industry-grade monitoring. 
    - Another very crucial observability tool in the Microservice world is Tracing.  Zipkin/Jaeger offers excellent tracing support for Microservices.
    - 可观测性
6. Unified Tech Stack
    - Microservice Architecture tells us that for a Microservice, take the programming language and framework best suitable for that microservice. This statement should not be taken literally.
    - 虽然微服务是语言无关的，但是多种技术栈会对于微服务治理带来巨大的问题。
7. Asynchronous Communication
8. Microservice First
    - In their opinion, once the Project became mature enough, the “nicely” designed Monolith can easily be transformed into Microservices. However, in my opinion, this approach will fail in most cases. In practice, the modules inside Monolith will be tightly coupled which will make it difficult to transform it into Microservices.
    - 作者对于单体first思潮给予了一定程度上的否定，臆想中的"nicely"到最后可能是"hardly"
9. Infrastructure over Libraries
    - instead of investing heavily in a language-specific library (e.g. Java based Netflix OSS), it is wiser to use frameworks (e.g. Service Meshes, API gateway).
10. Organizational Considerations
    - Almost 50 years ago (1967), Melvin Conway gave an observation that the Software Architecture of a company is limited by Organizational Structure (Conway’s Law) Although the observation is 50 years old, MIT and Harvard Business School have recently found that the law is still valid in modern days.