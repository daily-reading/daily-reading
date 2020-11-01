[Article Link](https://medium.com/airbnb-engineering/building-services-at-airbnb-part-4-23c95e428064) Building Services at Airbnb, Part 4

# Without API testing infrastructure

1. Breaking API Changes Silently

   The downstream microservice knows nothing when upstream changes its API and calls the changed API as usual. Bang!

2. Lack of Usable API Mock Data for Service Consumer

   Engineers experience unnecessarily heavy weight for test setup, which is counter-productive and make it less possible to cover enough logics and test cases within a limited time

3. Lack of Assurance for Mock’s Semantic Correctness

   Though engineers spend A LOT of time on create test doubles for unit / integration tests, we still cannot assure that the way we understand and the behavior we imitate is correct, which makes the test result less meaningful.

4. Lack of Validation for Service Owner

   Service owners has to set up validations against their API endpoints that could be different from the set of validations used by the API consumers, which again makes the test result less meaningful.

5. Lack of Real Time Metrics for API Test Quality

   For API tests, there is no metric equivalent to code coverage that is a widely used metric for unit tests, which brings almost zero insight of API test coverage and test data quality. 

# What infrastructure can do for service API testing

### **Static API Schema Validation**

Backwards Compatibility Check focuses on detecting potential API breaking changes

- We need this! One day, we will get one 
- Shall we plan to do this next year?

Schema Linter aims at enforcing our IDL best practices

- We have this :)
- Still needs to improve though. CI pipeline is way too slow now.

We only have check for syntax and best practices. Do we have API schema already? Do we have solid defination and describe of API inputs & outputs?

#### COULD-DO

*Promote current CI to put into use among all; Consolidate, materialize and manage all API (uniform CI process could help); Integrate API schema breaking smells into static check*

### Schema-based API Data Factory

Defining language-agnostic, schema-validated request and response fixtures to simplify the process of creating API data for mocking and testing

- Base for automate API unit / integration / End to End test
- Base for build real time metrics for API testing
- Make test data reusable under scenarios of unit / integration / End to End test
- Service owners define and maintain configs for test data
- Define API first, then develop, and verify at the end

#### COULD-DO

*Build Data factory upon API schema; Automated test data generation; Metrics relate to test data coverage*

### **API Mocking Framework**

Embedded into core service framwork that is parallel to the normal API interaction under production mode. 

- Provides simulated near real-world API interaction

- No need to use Mockito to mock API call
- Active mocking mode through config / Http header

Feels like we already have VENV as the foundation to differenciate calls from test / production.

COULD-DO

*Embed API Data Factory into mocking framwork; Embed mocking framwork into service framework; Hide usage details from users*

### **API Integration Testing Framework**

Ultimate infrastruture for API testing. Must based on the above layers: 

------------

API Integration Testing Framework

⬆︎

API Mocking Framwork

⬆︎

API Data Factory

⬆︎

API schema

------------

**Allows services to validate themselves with predefined test data**

- Automated and triggered within CI/CD process
- Enabled light weight end to end test without extra overhead for setup
- Provides real time API test metrics: test coverage / success rate / scope of validation
- Insert into CI/CD process as a stage

好大一个饼，闻起来很香，感觉吃着也会很香，就是做起来事情很多很复杂，不知道我们啥时候会做类似的事情。感觉是一个以年为单位做的事……
