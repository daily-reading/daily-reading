Defination:
1. Dependability = availability + Reliability
2. Availability = is the service down or not?
3. Realibility = is the service of good quality?
4. Fault tolerance = dependable or not

stateless component -> focus on availability
stateful component -> focus on realibility

Failure is common. The goal is to make system working under different types of failures.

Stateless service
1. it doesnt depend on state, so each component acts on its own
2. redudant elements are available in case of failure
3. load balancer selects a replacement to satisfy request

more questions:
how much redudancy?
1. complexity and cost of operation.
2. does it worth that redudancy?

adding redudancy doesnt always work. Thera are possible external issues as well.

Stateful service
1. each operation must be performed on multiple stores.
Q: in parallel?? or only one is doing the real work but others are standby?
2. replicate updates occur as a transaction.

a mechanisms must exist to ensure reassigning the work to redudancy is completed.
This requires a reliable messaging system.

Architecture for reliable stateful service
1. stateful role placement
    a. hash ring
    b. consistent placement algorithm decides the placement of resource
2. how?
    a. once failure is detected, use the hash for the new location
    b. resume service with the new location
Q: dont quite understand this part, does it mean the hashing decides the location of the set of redundants/resources, so when reassigning to the new resource, all the dependency will be in the same location?

3. channel persistentce layer (messaging)
placing redundant components in different AZs to ensure the failures are independent.
Q: are they running in sequence or in parallel? what's the perf?

Resource availability issues
套娃，what if the failure tolerant system is out of resources?


