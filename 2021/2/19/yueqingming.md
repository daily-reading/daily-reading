[Fast and Reliable Schema-Agnostic Log Analytics Platform](https://eng.uber.com/logging/)

ELK 架构下的挑战：
Log Schema: 日志时一个半结构化的数据，ES 并不能很好的处理结构的变化。
Operational Cost：为了适应规模的扩展，维护了大量的 ES 和 LogStash 集群。
Hardware Cost:消耗大量硬件资源
Aggregation:在实际的日志分析中，经常使用聚合查询，但是 ES 并不是为了聚合查询而设计，导致在某些大数据量的场景下，性能表现不佳。

### Introducing a New Schema-Agnostic Log Analytic Platform

关键的需求：

功能：

1. Schema-agnostic for developer productivity
2. Efficient support of aggregation queries
3. Support multi-region and cross-tenant queries

效率和维护：

1. Support multi-tenant properly to consolidate deployment
2. Reduce cost and be able to handle 10X scale
3. Improve reliability and simplify operation

兼容原来的 ELK 平台，用户可以继续使用 Kibana 进行日志分析

### Schema-agnostic data model

自动发现日志中的类型，并记下对应的时间戳，方便后续更新。

![f306a7515c75295f15fca6d214a8f45f.png](evernotecid://7F01B206-A616-4EA4-AABF-0C5FA9B75355/appyinxiangcom/26825047/ENResource/p272)

### ClickHouse table schema

1. 直接把日志作为字段存储在一个列中（查询速度慢）
![0e3631eedb533b5c889a6e43f4910840.png](evernotecid://7F01B206-A616-4EA4-AABF-0C5FA9B75355/appyinxiangcom/26825047/ENResource/p273)
2. 将所有字段+类型作为一个字段展开（查询快，但是规模膨胀较快，不具有扩展性）
![3bc7539a2e7df3594694ae9b29012177.png](evernotecid://7F01B206-A616-4EA4-AABF-0C5FA9B75355/appyinxiangcom/26825047/ENResource/p274)
3. 同一类型的所有字段和值分别存在一个数组中，查询的时候使用数组访问函数进行访问。（访问速度和规模扩展的折中）
![fd810d01fe310597df19415a9dcf059b.png](evernotecid://7F01B206-A616-4EA4-AABF-0C5FA9B75355/appyinxiangcom/26825047/ENResource/p275)

### ingest all and query any,fast

![c20cf7c52339ab51af1d9dffd77c9094.png](evernotecid://7F01B206-A616-4EA4-AABF-0C5FA9B75355/appyinxiangcom/26825047/ENResource/p280)

**Schema-free ingestion**

Log Ingester 在批量写入 ClickHouse之后才会响应 ACK，保证至少一次写入。在写入过程中解析日志 Schema，然后存储。

**Type-aware query**

要求用户了解 ClickHouse 的表结构，然后书写 SQL 来进行日志查询，对用户不太友好。所以需要提供一个友好的查询机制，以及该机制和 ClickHouse 查询之间的映射。

**Query interface**
![e105ee578010b624a23cd787c6336f5d.png](evernotecid://7F01B206-A616-4EA4-AABF-0C5FA9B75355/appyinxiangcom/26825047/ENResource/p281)
查询时进行查询映射，依据元数据和查询条件生成对应的 ClickHouse SQL。

**Adaptive indexing**

为某些常访问的字段添加索引 28 定律

### Reliability,Scalability,Multi-regiion and Multi-tenant
![91578ec06aa2c724e75531559677211a.png](evernotecid://7F01B206-A616-4EA4-AABF-0C5FA9B75355/appyinxiangcom/26825047/ENResource/p282)

查询和数据分离，元数据在查询节点之间同步。

### Future work 
SQL 是查询的终态？

