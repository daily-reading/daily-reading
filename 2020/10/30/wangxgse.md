# 提交说明
- 编号：NO.16@20210418
- 文章：[Building Services at Airbnb, Part 4](https://medium.com/airbnb-engineering/building-services-at-airbnb-part-4-23c95e428064)

## challenges regarding microservices testing.
1. Breaking API Changes
2. Lack of Usable API Mock Data for Service Consumer
    - > Without support, engineers are duplicating time and effort to create mock data, clients, and services repeatedly in different places.
    - > It also leads to engineers experiencing unnecessarily heavy weight test setup. For example, spinning up dependent service and even dummy databases. Those heavy efforts are counter-productive. Tests are supposed to be light and frequently run.
3. Lack of Assurance for Mock’s Semantic Correctness
4. Lack of Validation for Service Owner
5. Lack of Real Time Metrics for API Test Quality
6. Most Importantly: Test in a Lightweight and Scalable Manner
    - > Note that, there are so many critical components needed to make a successful testing story, organization-wise education and best practices, a scalable test runner, continuous integration infrastructure(CI), continuous delivery infrastructure(CD), testing environments, etc. In this post, we will be only focusing on the schema aspect of service API testing.
    
## schema-oriented infrastructure
1. Static API Schema Validation
2. Schema-based API Data Factory
3. API Mocking Framework
4. API Integration Testing Framework (AIT)
- > API Integration Testing Framework (AIT) is a framework that allows services to validate themselves with predefined test data in production environments without writing boilerplate, and that ensures semantic correctness of mock data.