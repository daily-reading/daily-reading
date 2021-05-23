why layers?
总而言之，就是decouple，ownership, responsibility, scope等等等等的decouple。分工明确，降低重构，升级，管理的cost

3 layers architecture
1. poresentation: UI
2. business: 业务逻辑
3. data access: access data, e.g. Entity Framework (perf巨差，别用)

Note:
想起了之前看的那篇BFF，多出来的那一层，可以算BUSINESS还是处于1和2之前多的一层？但连gateway也是属于business layer，那应该也是属于business layer的吧。