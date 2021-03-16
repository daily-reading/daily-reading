# 提交说明
- 编号：NO.11@20210312
- 文章：[What Does Cloud Native Really Mean?](https://medium.com/swlh/what-does-cloud-native-really-mean-1b10ed003aa9)

# 读书笔记
## 概述
> cloud native means not just moving to cloud, but fully leveraging the uniqueness of cloud infrastructure and services to rapidly deliver business value.

Cloud native 在IT层面的目标
- Agility and Productivity: 
> Enable rapid innovation that is guided by business metrics. De-risk maintenance and keep environments current.
- Resilience and Scalability: 
> Target continuous availability that is self-healing and downtime-free. Provide elastic scaling and the perception of limitless capacity.
- Optimization and Efficiency: 
>Optimize the costs of infrastructural and human resources. Enable free movement between locations and providers.

Cloud native最终服务于两大目标
- IT Goals：如上
- Business Outcomes
  - Market growth
  - Risk Mitigation
  - Cost Reduction

Cloud native包括三个层面
- architecture & design,
- technology and infrastructure,
- people & process

## Technology & infrastructure
> What is “cloud” in the context of “cloud native”

> A decade or more ago the term “cloud” was largely about location. It usually referred to infrastructure located in someone else’s data center accessible over the internet. However today “cloud” is more a statement about how you interact with that infrastructure. 

cloud更多的意味着如何与基础设施进行交互，在交互时，提供如下能力：
- Self-provisioning: Requisition of new virtual resources (servers, storage, networking) instantly.
- Elasticity: Automatically scale resources (and their associated costs) up and down based on demand.
- Auto-recovery: Resources are designed to recover from failures without intervention, and with minimal impact on service availability.

cloud意味着对底层基础设施的抽象
- Immutable deployment — e.g. container image based deployment
- Declarative provisioning — “infrastructure as code” providing a to-be state
- Runtime agnostic — The platform sees components (e.g. containers) as black boxes, with no need to understand their contents
- Component orchestration — Enable management (monitoring, scaling, availability, routing etc.) through generic declarative policy and provisioning.

## Architecture and Design
> What do we mean by “native” in “cloud native”?

> By “native” we mean that we will build solutions that don’t just “run on cloud”, but specifically leverage the uniqueness of cloud platforms. Applications don’t just magically inherit the benefits of the underlying cloud infrastructure, they have to be taught how.

> We need to write our solutions to ensure, for example, that they can scale horizontally, and that they can work with the auto-recovery mechanism.
- minimize statefulness,
- reduce dependencies,
- have well defined interfaces,
- are lightweight,
- are disposable

## People and Process
> How does “cloud native” change the way we organize and work?

Microservices approach
- Microservices implies that you are building your services in small, autonomous teams. 
- Microservices also implies that you are using agile methods and applying DevOps principles to your development processes.
- DevOps requires you to adopt other specific technical processes such as automated testing (perhaps including test driven development), and strongly leads you toward trunk-based development. 

container technology
- This radically reduces the learning curve for people working across multiple technology areas and enabling broader role and knowledge sharing increasing efficiency and reducing cost
- Containers, and specifically container image technology simplify the automation of CI/CD pipelines, leading to shorter build/release cycle time, and increasing productivity. 
- Immutable container images combined with declarative “infrastructure as code” increase the consistency of deployment across different environments. This reduces testing and diagnostics costs, improves speed of deployment, and reduces downtime. 
- From a process perspective this enables a “shift left” of aspects such as reliability, performance and security testing. This in turn enables a more DevOps/DevSecOps culture, where developers have more accountability for the operational qualities of the code.

## Summarizing what it means to be “cloud native”
云原生包括三个方面：
- Platforms that abstract complexities of the infrastructure. (Infrastructure and Technology)
- Solutions that make best use of the infrastructure abstractions (Architecture and Design)
- Automation of development, operations and business processes, and increasing autonomy of development teams (People and Process)
