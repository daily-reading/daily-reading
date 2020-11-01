# Building Services at Airbnb, Part 4
提高engineering 生产力和质量的挑战
## Why schema-oriented infrastructure is critical for service testing
1. Breaking API Changes
  - API要确保不再被使用才下线。想到了最近我们讨论的方案先@Desprate,几个月后在下线
  - client端先更新deploy，更新API的server端再delpoy
2. Lack of Usable API Mock Data for Service Consumer
  - 没有组件支持的话，工程师面对不同场景测试需要重复构造大量mock数据
  - 工程师也要体验非常的繁重的setup，这是非常不利于生产的
3. Lack of Assurance for Mock’s Semantic Correctness
  - 无法保证mock数据语义正确
  - 商业逻辑一直在编，人工维护的mock可能会过时
4. Lack of Validation for Service Owner
  - 框架要支持Validation
5. Lack of Real Time Metrics for API Test Quality
  - test coverage and test data quality

## How service owners and consumers benefit
提供了一些框架
1. Static API Schema Validation
- Backwards Compatibility Check focuses on detecting potential API breaking changes.
- Schema Linter aims at enforcing our IDL best practices.

- 禁止不向后兼容的schema修改
- 实现  Abstract Syntax Tree
- 接入CICD

2. Schema-based API Data Factory
- Mocking dependent service API in unit tests and integration tests.
- Validating API endpoint correctness using the materialized API request/response.

- mock数据与语言无关，通用
- CI检测数据是否与最新API兼容

3. API Mocking Framework
- 模拟调用
- latency

4. API Integration Testing Framework 
benefit
- Producer service is validated without writing boilerplate code
- Mocking data is semantically validated
- Provide realtime API test coverage instrumentation
- Pluggability and Lightweight
