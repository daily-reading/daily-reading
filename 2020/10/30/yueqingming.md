[Building Services at Airbnb,Part 4](https://medium.com/airbnb-engineering/building-services-at-airbnb-part-4-23c95e428064)

### 微服务测试的几个挑战

* API 的兼容性
* 服务消费者缺乏可用的 Mock 数据
* 测试的 Mock 语义缺乏正确性保证
* 没有服务的参数校验
* 没有API 的测试度量指标（例如代码覆盖率）


测试工具的两个方面：
服务 Owner：

* Static API Schema Validation
    * API 兼容性检查
    * IDL 最佳实践检查
* API Integration Testing Framework
    * 各种检查最好都用工具实现

服务 Consumer：
* API Mocking Framework
* API Integration Testing Framework 


API Mock Features:
1. Ubiquitous API Mocking: Client or Service Side
![f8f27efa687a5b6d9b8f3c709e4e2ca3.png](evernotecid://7F01B206-A616-4EA4-AABF-0C5FA9B75355/appyinxiangcom/26825047/ENResource/p256)
2. Dependency Isolation for Shallow Integration Tests
3. various Mocking Options as Needed
* Matching
* Stubbing
* Latency Simulation






