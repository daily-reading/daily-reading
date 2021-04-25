## # The BFF Pattern (Backend for Frontend): An Introduction

原文：https://blog.bitsrc.io/bff-pattern-backend-for-frontend-an-introduction-e4fa965128bf

BFF 架构解决的问题：我们微服务返回给前端的数据，不符合前端格式化要求。这样前端会占用浏览器资源去做逻辑进行数据过滤和格式化，影响用户体验。

BFF 架构做的工作：BFF相当于浏览器与多个微服务之间的代理，它通过请求多个相关APIs 获取数据，然后根据前端需要，将数据过滤、格式化成前端所需的数据返回给前端。

BFF 最佳实践：
1.BFF是微服务和客户端的转换层，自己的API实现应该放在微服务层中。
2.避免BFF逻辑重复，单个BFF应该迎合特定的用户体验，而不是设备类型。
3. 避免过度依赖BFF-BFF只是一个转换层，API层和前端都应该注意功能和安全性问题。