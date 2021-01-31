origin: http://www.cs.utah.edu/~stutsman/cs6963/public/papers/memcached.pdf



### challenge
- largest social network in the world
- multiple geographically distributed clusters

### content included
- evolution of Facebook's memcached-based architectrue
- enhancement to memcached that improve performance and increase memory effciency
- mechanisms that improve the operating ability as scale
- workload benchmark 

### Overview
- query cache
  - to lighten the read load on databases
  - demand-filled & look-aside cache
  - delete cached data instead of updating it because deletes is idempotent(幂等)
- generic cache
  - as key-value store, for example the pre-computed data from ML

- cluster
  - memcached provides no server-to-server coordinates, just a isolate node in cluster
  - facebook build a branch of config, aggregation, routing service to organize memcached instance to a distributed system

- design goal trade-off
  - the change that impact user or operating, the limited optimize will rarely considered
  - we sacrifice a little consistency for avoiding service from excessive load. permit stale is a param to choose


### In a Cluster, Latency and Load

the cluster is large, we need to focus on reducing the latency of fetching data and the load imposed due to cache miss

#### Reducing Latency
- reading fan-out is large, a user web reqeust can result in hundreds of single memecached get requests
- a single server may be the bottleneck for server latency
- how to solve?
  - focus on client
    -  the client will handle all local action such as serialization, compression, routing, error handling, and batching. 
    - client will hold a map of all avaliable servers, which is query from auxiliary(辅助) conﬁguration system.
  - Parallel requests and batching
    - maintain a DAG to represent dependencies between data to maximize the number of items that can be feched concurrently
  - client-server communication
    - servers do not communicate with each other. embed the complexity into the stateless client rather than memcached server
      - rapit iteration
      - simplify our deployment process
      - mcrouter to do as a proxy to communicate, handle the rouing things
    - read over UDP rather than TCP
      - UDP connectless
      - communicate with memcached server directly bypassing *mcrouter*
      - do not establishing connection reduce the overhead
      - see packets drops as a fetch faild or cache miss
      - do not inserting cache after figureing out why the fetch failed(packet drops for example)
    - *set* and *delete* operation over TCP
    - Incast congestion(拥塞控制)

#### Reducing Load
- Lease
- 

