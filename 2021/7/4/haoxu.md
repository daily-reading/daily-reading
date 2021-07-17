Start from scratch
1. single server + single database
2. disadvantages: either fails, system fails

Scalability: to handle more volumes by adding more resources
scale up: verticle scaling, adding more RAM and CPU to existing server
scaleup is good for small systems but with limitations
1. every simple system has a limit
2. downtime is unavoidable during upgrade
3. may be costly

scale out: horizontal scaling, adding more entities to the pool of resources.
1. need to consider it before the system is built
2. more resources need to be miantained
3. distribution system

load balancer
1. between client and server
2. fault tolerance

policies:
1. round robin
2. least number of connections
3. fastest response time
4. weighted
5. IP hash

scaling a relational database. additional reading: https://docs.microsoft.com/en-us/azure/architecture/best-practices/data-partitioning
1. replication
2. federation
3. sharding
4. denormalization
5. sql tuning

master-slave replication
bottleneck
1. if master is down, it will fall into issues
2. additional algorithm to promote a slave to a master

solution
1. synchronous: dont commit until accepted by all servers
2. async: commit -> delay -> propagation

master-master replication
pro:
1. if one master fails, another one can take over.
2. master can be distributed
3. limited by the ability of the master to process updates.

Federation
split different tables into different sub-databases.

Sharding
split different rows/cols into different databases
Horizontal partitioning: by rows
Vertical partitioning: by cols
Need a lookup serivce
cons:
1. joins become more expensive
2. could compromise integrity
3. schema change is expensive
4. if distribution is not uniform, load can be high

Denormalization
increase read perf at the cost of write.

which DB to use?
SQL: everyone knows it
No SQL: 
1. key value stores: redis
2. document database: mongoDB
3. wide column db
4. graph DB
5. blob db

which one to use? DEPENDS

scaling the web tier horizontally
1. no stateful architecture

caching
CDN

More on the way