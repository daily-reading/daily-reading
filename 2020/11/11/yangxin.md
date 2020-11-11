# Taming Service-Oriented Architecture Using A Data-Oriented Service Mesh
## Massive SOA Dependency Graphs
- 微服务依赖图很复杂
- spaghetti SOA
> To help manage the larger number of services inherent in a microservices-based architecture, we need organizing principles as well as technical measures to implement those principles. At Airbnb, we undertook an effort to find such principles and measures. 
- principles and measures，如果没有measure其实很多principles都会无疾而终

## Procedure- vs Data-Oriented Design
- 为啥叫Data-Oriented，感觉有点像是Object-Oriented    
  - define classes of objects     
  - encapsulate an internal representation of an object accessed via a public API of methods on the object
- 作者认为SOA是倒退到了procedure-oriented

## Viaduct: A Data-Oriented Service Mesh
- 感觉mesh centor需要做一些分发和聚合的操作，并且要维护schema
- 这么一搞依赖图确实简洁很多，但是感觉是把维护依赖的复杂性从service端转移到centor统一管理。
https://www.zhihu.com/question/264629587
