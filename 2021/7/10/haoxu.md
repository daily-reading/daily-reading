benefit: for expensive operations, run in background to avoid blocking the main thread. better UX, reliability.

ability:
1. reliability
2. scalability
3. isolation
4. time accuracy
5. efficient queuing
6. useability
7. unscheduling

Resque issues
1. at most once, not reliable
2. scaling bottleneck
3. share queues
4. limited scheduling ability
5. the solution to address the limits is not ideal

SQS
1. simple and scale up
2. high throughput and low enough latency
3. at least once delivery
4. many other features

immediate jobs
depends on SQS, wrap it. same API.

delayed jobs
1. queue jobs into the inbound queue, so it wont lost
this makes Dynein stateless

delayed job scheduler
Quartz issues
1. scalability
2. usability
3. queueing

 DynamoDB Based Scheduler
 "These steps are the only feature that the service-facing Dynein instances will handle, and the actual schedulers are run on different Kubernetes Pods."
 with randomID, sortKey and status

 