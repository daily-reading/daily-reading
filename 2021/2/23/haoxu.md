FOQS consists of
1. producer (who enqueues items)
2. consumer (who dequeues items)

use cases
1. async operations
2. vide encoding
3. translation
etc.

* sounds similar to azure service bus???

PQ item properties
1. namespace
2. topic
etc.

topic->PQ
namespace->PQ use case, host of topics

enqueue:
1. enqueue request is a promise
2. enqueue worker inserts to shards
3. circuit breaker design for unhealthy shards

dequeue
1. items are sorted by priority and deliver_after
2. lease. items can be set to be delivered at most onece or at least once. Items will be delted or available for redelivery after lease expires
3. Prefetch Buffer for item with highest priority. Cache for performance

ACK/NACK
1. shard id is part of item id
2. route to shard specific host

characters of workloads
1. e2e processing delay
2. rate of processing
3. priorities
4. location

FOQS uses pull model and it solves discoverability with a routing layer component.

disaster readiness
1. replications
2. enqueue forwarding
3. global rate limit