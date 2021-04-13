## 提交编号
NO.10@20210307

## 文章
[On Designing and Deploying Internet-Scale Services](https://www.usenix.org/legacy/event/lisa07/tech/full_papers/hamilton/hamilton.pdf)

## 读书笔记

本文讲了系统设计的方方面面，很多地方都需要反复去读，反复去思考，是一个很好的设计清单。所有的设计要点的讨论中有三个基本原则：
- Expect failures
- Keep things simple
- Automate everything

### Overall Application Design
- Design for failure
- Redundancy and fault recovery
- Commodity hardware slice
- Single-version software
- Multi-tenancy
- Quick service health check
- Develop in the full environment
- Zero trust of underlying components
- Do not build the same functionality in multiple components
- One pod or cluster should not affect another pod or cluster
- Allow (rare) emergency human intervention
- Keep things simple and robust
- Enforce admission control at all levels
- Partition the service
- Understand the network design
- Analyze throughput and latency
- Treat operations utilities as part of the service
- Understand access patterns
- Version everything
- Keep the unit/functional tests from the previous release
- Avoid single points of failure

### Automatic Management and Provisioning
- Be restartable and redundant
- Support geo-distribution
- Automatic provisioning and installation
- Configuration and code as a unit
- Manage server roles or personalities rather than servers
- Multi-system failures are common
- Recover at the service level
- Never rely on local storage for non-recoverable in- formation
- Keep deployment simple
- Fail services regularly

### Dependency Management
- Expect latency
- Isolate failures.
- Use shipping and proven components
- Implement inter-service monitoring and alerting
- Dependent services require the same design point
- Decouple components

### Release Cycle and Testing
- Ship often
- Use production data to find problems
   - Measurable release criteria
   - Tune goals in real time
   - Always collect the actual numbers. 
     - Collect the actual metrics rather than red and green or other summary reports. Summary re- ports and graphs are useful but the raw data is needed for diagnosis.
   - Minimize false positives.
     - People stop paying attention very quickly when the data is incor- rect. It’s important to not over-alert or opera- tions staff will learn to ignore them. This is so important that hiding real problems as collateral damage is often acceptable.
   - Analyze trends.
   - Make the system health highly visible
   - Monitor continuously.
     - It bears noting that people must be looking at all the data ev- ery day. Everyone should do this, but make it the explicit job of a subset of the team to do this.
- Invest in engineering. 
  - Good engineering mini- mizes operational requirements and solves prob- lems before they actually become operational is- sues. Too often, organizations grow operations to deal with scale and never take the time to engineer a scalable, reliable architecture. 
  - Services that don’t think big to start with will be scram- bling to catch up later: 那些起初没考虑过的运维问题的服务，往往后面都会变的手忙脚乱。
- Support version roll-back
- Maintain forward and backward compatibility. 
  - This vital point strongly relates to the previous one. Changing file formats, interfaces, logging/ debugging, instrumentation, monitoring and con- tact points between components are all potential risk. Don’t rip out support for old file formats until there is no chance of a roll back to that old format in the future.
- Single-server deployment.
  - This is both a test and development requirement. The entire ser- vice must be easy to host on a single system. Where single-server deployment is impossible for some component (e.g., a dependency on an external, non-single box deployable service), write an emulator to allow single-server testing. Without this, unit testing is difficult and doesn’t fully happen. And if running the full system is difficult, developers will have a tendency to take a component view rather than a systems view.
- Stress test for load.
  - Run some tiny subset of the production systems at twice (or more) the load to ensure that system behavior at higher than expected load is understood and that the sys- tems don’t melt down as the load goes up.
- Perform capacity and performance testing prior to new releases. 
  - Do this at the service level and also against each component since work load characteristics will change over time. Problems and degradations inside the system need to be caught early.
- Build and deploy shallowly and iteratively. 
  - Get a skeleton version of the full service up early in the development cycle. This full service may hardly do anything at all and may include shunts in places but it allows testers and devel- opers to be productive and it gets the entire team thinking at the user level from the very beginning. This is a good practice when build- ing any software system, but is particularly im- portant for services.
- Test with real data
- Run system-level acceptance tests. Tests that run locally provide sanity check that speeds it- erative development. To avoid heavy mainte- nance cost they should still be at system level.
- Test and develop in full environments
 
### Hardware Selection and Standardization
- Use only standard SKUs
- Purchase full racks
- Write to a hardware abstraction
- Abstract the network and naming

### Operations and Capacity Planning
- Make the development team responsible
  - If development is frequently called in the middle of the night, automation is the like- ly outcome. If operations is frequently called, the usual reaction is to grow the operations team.
- Soft delete only
  - Never delete anything. Just mark it deleted.
- Track resource allocation
  - Whatever the metric, there must be a direct and known correla- tion between this measure of load and the hard- ware resources needed.
- Make one change at a time. 
  - When in trouble, only apply one change to the environment at a time
- Make everything configurable.
  - Anything that has any chance of needing to be changed in produc- tion should be made configurable and tunable in production without a code change. Even if there is no good reason why a value will need to change in production, make it changeable as long as it is easy to do.

### Auditing, Monitoring and Alerting
> Any time there is a configuration change, the ex- act change, who did it, and when it was done needs to be logged in the audit log. When production problems begin, the first question to answer is what changes have been made recently. Without a configuration au- dit trail, the answer is always ‘‘nothing’’ has changed and it’s almost always the case that what was forgotten was the change that led to the question.
> Alerting is an art. There is a tendency to alert on any event that the developer expects they might find interesting and so version-one services often produce reams of useless alerts which never get looked at. To be effective, each alert has to represent a problem. Otherwise, the operations team will learn to ignore them. We don’t know of any magic to get alerting cor- rect other than to interactively tune what conditions drive alerts to ensure that all critical events are alerted and there are not alerts when nothing needs to be done.

* Instrument everything. Measure every customer
    * interaction or transaction that flows through the system and report anomalies.
* Data is the most valuable asset.
    * Lots of data on what is happening in the system needs to be gathered to know it really is working well.
* Have a customer view of service.
    * Perform end-to-end testing. Runners are not enough, but they are needed to ensure the service is fully working. Make sure complex and important paths such as logging in a new user are tested by the runners.
    * Avoid false positives. If a runner failure isn’t con- sidered important, change the test to one that is. Again, once people become accustomed to ignor- ing data, breakages won’t get immediate attention.
* Instrument for production testing.
    * In order to safely test in production, complete monitoring and alerting is needed. If a component is fail- ing, it needs to be detected quickly.
* Latencies are the toughest problem.
    * Examples are slow I/O and not quite failing but process- ing slowly. These are hard to find, so instru- ment carefully to ensure they are detected.
* Have sufficient production data. (In order to find problems, data has to be available. Build fine- grained monitoring in early or it becomes ex- pensive to retrofit later.)
    * Use performance counters for all opera- tions
        * - Record the latency of operations and number of operations per second at the least.
    * Audit all operations.
        * - Every time somebody does something, especially something sig- nificant, log it. This serves two purposes: first, the logs can be mined to find out what sort of things users are doing (in our case, the kind of queries they are doing) and second, it helps in debugging a prob- lem once it is found.
        * - A related point: this won’t do much good if everyone is using the same account to ad- minister the systems. A very bad idea but not all that rare.
    * Track all fault tolerance mechanisms. 
        * - Fault tolerance mechanisms hide failures.
    * Track operations against important entities. 
    * Make an ‘‘audit log’’ of everything signifi- cant that has happened to a particular entity
    * Asserts. 
        * Use asserts freely and throughout the product. Collect the resulting logs or crash dumps and investigate them. 
        * For systems that run different services in the same process boundary and can’t use asserts, write trace records. 
        * Whatever the implementation, be able to flag problems and mine frequency of different problems.
    * Keep historical data. 
        * - Historical performance and log data is necessary for trending and problem diagnosis.
* Configurable logging. 
    * - Support configurable log- ging that can optionally be turned on or off as needed to debug issues. Having to deploy new builds with extra monitoring during a failure is very dangerous.
* Expose health information for monitoring. 
    * - Think about ways to externally monitor the health of the service and make it easy to monitor it in production
* Make all reported errors actionable.
    * - If an unrecoverable error in code is detected and logged or reported as an error, the error message should indicate possible causes for the error and suggest ways to correct it
    * - Un-actionable error reports are not useful and, over time, they get ignored and real failures will be missed.
* Enable quick diagnosis of production problems.
    * Give enough information to diagnose. 
    * Chain of evidence. : Make sure that from beginning to end there is a path for developer to diagnose a problem. This is typically done with logs.
    * Debugging in production.
        * 这个有点奇怪，好像也没什么好处
        * We prefer a model where the systems are almost never touched by anyone including operations and that de- bugging is done by snapping the image, dumping the memory, and shipping it out of production.
    * Record all significant actions.
        * 记录那些重要的、关键的操作
        *  Every time the system does something important, particu- larly on a network request or modification of data, log what happened. This includes both when a user sends a command and what the system internally does. Having this record helps immensely in debugging prob- lems. Even more importantly, mining tools can be built that find out useful aggregates, such as, what kind of queries are users doing

### Graceful Degradation and Admission Control
* big red switch
    * The concept of a big red switch is to keep the vi- tal processing progressing while shedding or de- laying some non-critical workload The key thing is deter- mining what is minimally required if the system is in trouble, and implementing and testing the option to shut off the non-essential services when that happens.
* Control admission
* Meter Admission
    * It’s vital that each service have a fine-grained knob to slowly ramp up usage when coming back on line or re- covering from a catastrophic failure.
    * Where a service has clients, there must be a means for the service to inform the client that it’s down and when it might be up.
    * This also gives an opportunity for the service owners to communi- cate directly with the user (see below) and con- trol their expectations. Another client-side trick that can be used to prevent them all syn- chronously hammering the server is to intro- duce intentional jitter and per-entity automatic backup.

### Customer and Press Communication Plan
* Systems fail, and there will be times when laten- cy or other issues must be communicated to cus- tomers. Communications should be made available through multiple channels in an opt-in basis: RSS, web, instant messages, email, etc. For those services with clients, the ability for the service to communicate with the user through the client can be very useful. The client can be asked to back off until some specific time or for some duration. The client can be asked to run in disconnected, cached mode if supported. The client can show the user the system status and when full functionality is expected to be available again.
* Customer Self-Provisioning and Self-Help