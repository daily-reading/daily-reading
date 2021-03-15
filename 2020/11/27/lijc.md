# Edgar: Solving Mysteries Faster with Observability

做一个 metrics、tracing、logging 关联的排障平台，要以 tracing 为核心，因为 tracing 是这三者中唯一有**因果关系**的(metrics 是聚合值，logging 是信息丰富的 event)。

1. 需要统一元信息，以实现跨系统查询
   1. 即 tracing、logging 需要统一 [timeRange、service、traceId、spanId] 这组元信息(将 logging 信息整合进 tracing，traceId + spanId，只有 traceId 不够)
   2. 即 tracing、metrics 需要统一 [timeRange、service、endPoint(接口)] 这组元信息
   3. 即 metrics、logging 需要统一 [timeRange、service、endPoint(接口)] 这组元信息(文章没涉及这个，我想的)
2. 要尽量在一个界面里展示全部信息(traicng、log、metrics、analysis)，减少切换，同时这个界面易于共享

相对于通用 tracing、metrics、logging 系统，这个平台需要一些独特的功能

1. tracing 需要能够采集到的全部有价值的 trace：慢 trace、错 trace 等
2. logging 需要将 traceId、spanId 打印出来
3. 查询的数据长期存储。
