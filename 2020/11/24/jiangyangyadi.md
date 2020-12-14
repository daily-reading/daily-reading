[Artical](https://www.usenix.org/legacy/event/lisa07/tech/full_papers/hamilton/hamilton.pdf) **On Designing and Deploying Internet-Scale Services**

This article is quite long and mentions a lot of best practices are reasonable and helpful for designing and developing operations-friendly services. I could summarize all points the author wanted to clarify, but I think it would better to choose some of them that are more useful for me at this moment. May be when I read this artical again 1 year later,  I will have a completely different point of view :)

## Principles

1. Expect Failures

   *A component may crash or be stopped at any time*

2. Keep Things Simple

   *Simple things are easier to get right*

3. Automate Everything

   *People make mistakes and need sleep*

## Best Practices

- **Design for failure**

  - If a failure requires any immediate artificial action, the service will not scale cost-effectively and reliably
  - Failure recovery must be a very simple path and that path must be tested frequently, otherwise they will not work when needed
  - 简单自动化的故障恢复手段，需要经常演练

- **Redundancy and fault recovery**

  - Whenever a failure happens on an arbitrary sever, the SLA should still hold
  - Brainstorming all possiple kind of failure, and make sure we have an acceptable level of loss

- **Single-version software**

  - The most economic way is: Don’t give customers control over the version they run, and only host one version
  - 引申到基础组件，用户与基础组件的强耦合让升级变得困难，同时要一直维护旧的版本

- **Develop in the full environment**

  - Developers should test not only the part changed by themselves, but also the whole system
  - A single-server deployment is needed to do so

- **Zero trust of underlying components**

  - Assume that underlying components will fail
  - Fallback strategy is needed

- **Keep things simple and robust**

  - Complicated al- gorithms and component interactions multiply the difficulty of debugging, deploying, etc
  - Especially in a high-scale service

- **Enforce admission control at all levels**

  - Authorization & flow control
  - Avoid overloading & gracefully degrading

- **Analyze throughput and latency**

  - Understand the pattern of throughput and latency over core services that helps catch issues and prepare better

- **Understand access patterns**

  - When planning new features, always consider what load they are goint to put on the whole system especially the databases

- **Avoid single points of failure**

  - Prefer stateless implementations, easy to scale out
  - Load balance over a group of servers

- **Configuration and code as a unit**

  - Treat them as a unit and only change them together are often more reliable
  - The unit is deployed by test in exactly the same way that it will be deployed to production

- **Fail services regularly**

  - Initiatively take down serivices will go a long way to exposing service, system, and network weaknesses
  - 暴露问题，故障演练

- **Expect latency**

  - Calls to external components may take a long time to complete.
  - Don’t let delays in one component cause delays in completely unrelated areas. 
  - Ensure all interactions have appropriate timeouts

- **Isolate failures**

  - Fail fast to prevent cascading failures

- **Implement interservice monitoring and alerting**

  - If the service is overloading a dependent service, the depending service needs to know
  - If the exhausted service can’t back-off automatically, alerts need to be sent

- **Ship often**

  - Less big-bang changes

- **Perform capacity and performance testing prior to new releases**

  - Problems and degradations inside the system need to be caught early
  - Plus, run some tiny subset of the production systems at twice (or more) the load to ensure that system will not melt down as the load goes up

- **Soft delete only**

  - Never delete anything, just mark it deleted, unless the old version has no chance to be rolled out
  - Always keep a rolling two week histroy available

- **Make one change at a time**

  - Multiple changes meant cause and effect could not be correlated

- **Make Everything configurable**

  - Anything that has any chance of needing to be changed in production should be made configurable and tunable in production without a code change

- **Make all reported errors actionable**

  - If an unrecoverable error in code is detected and logged or reported as an error, the error message should indicate possible causes for the error and suggest ways to correct it

- **Enable quick diagnosis of production problem**

  - Give enough infomation to diagnose when a problem is flagged
  - Debugging in production
  - Record all significant actions

- **Support “Big Red Switch”**

  - A ‘‘big red switch’’ is a designed and tested action that can be taken when the service is no longer able to meet its SLA, or when that is imminent
  - To shed non-criticak load in an emergency

- **Customer Self-Provisioning and Self-Help**

  - Provide self-service procedure
  - If a customer can go to the web, enter the needed data and just start using the service, they are happier than if they had to waste time in a call processing queue

  
