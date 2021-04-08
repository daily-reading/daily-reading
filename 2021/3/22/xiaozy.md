# Modern Best Practices for Testing in Java

> 原文地址 https://phauer.com/2019/modern-best-practices-testing-java/

## TL;DR

1. 写小、准确的测试，避免在一个测试函数中写多个 case
2. 写 self-contained 测试用例：组合优于继承
3. Write dumb tests：避免复用生成代码，而是比较输入输出
   1. 和 assert 状态而不是行为的思路比较类似
4. KISS > DRY
5. 测试应该足够贴近生产环境

## 通用原则

1. 测试结构, Given + When + Then
   1. Given(Input): 构造输入数据、setup mock
   2. When(Action)：调用被测试的逻辑
   3. Then(Output)：执行断言，来验证状态
2. 使用 actual/expected 前缀，来明确表名变量的含义
3. 使用固定的数据，而不是随机的数据

## 写小而准确的测试

1. 尽量使用字描述命名 Helper 函数，将细节和重复性代码抽到 Helper 函数里，例如创建数据
2. 不要滥用变量，减少过多变量带来的干扰，KISS > DRY，[see this](https://phauer.com/2019/modern-best-practices-testing-java/#dont-overuse-variables)
3. 一个测试方法不要测多个 case
4. 只断言与测试 case 相关的状态，牢记其他状态在其他测试函数中已经测过了

## Self-contained Tests

1. 不要隐藏相关的参数，创建数据和断言数据的参数都应该暴露出来，任何需要测试函数控制的行为和状态，都应该通过参数暴露出来
   > Define a parameter for everything that is important for the test and needs to be controlled by the test.
2. 在测试函数中引入测试数据，而不是在 `@Before` 方法里
3. 更倾向用组合而不是继承

## Dumb Test

1. 使用硬编码的值来比较输出
   1. 不用使用变量来和输出对比
   2. 比较输出值，而不是去断言内部的行为
   3. 不要比较生产代码的状态
   4. 不要复用生产代码的逻辑：可能会漏掉因为复用代码而引入的 bug，因为后续不会再测试了
   5. 不要重写生产代码的逻辑
   6. 不要写太多逻辑，测试代码应该关注输入和输出

## 测试应该贴近生产

1. 测试应该关注整个纵向切面（controller, logic, storage）的集成测试，而不是每个分层的测试
   1. 如果分开关注，测试代码会被重构破坏
   2. 需要维护多个测试
2. 不用使用内存数据库，而应该只用真正的数据库，因为内存数据库和实际生产中的数据库存在差异，无法保证实际生产环境的行为正确性

## Java/Jvm

1. 使用 `-noverify -XX:TieredStopAtLevel=1` 参数，来提升测试代码执行的速度
2. 使用 `AssertJ`
   - 类型安全
   - assert 齐全，方便写 assert
   - 错误信息自描述
3. 避免 `assertTrue` 和 `assertFalse`，错误信息不够友好

## 使用 Junit5

1. 使用参数化的测试：使用参数来指定不同的输入数据
2. 使用 `@Nested` 注解将测试函数分组，相关的测试 case 分到相同的组
3. 使用 `@DisplayName` 来提升测试方法名字的可读性

## 保证实现可测试

1. 永远不要使用 static 变量/方法
2. 参数化，通过参数来控制类的行为
3. 使用构造器注入
4. 不要使用实际的时间（`Instant.now()` or `new Date()`），而是 mock 时钟
5. 分离异步执行逻辑和实际业务逻辑

## 其他

1. mock 远程调用的结果，例如 OkHttp WebMockServer，WireMock, TestContainer MockServer
2. 使用 Awaitility 来言异步代码
   1. 将同步代码合异步代码分开测试
3. 不使用 Spring DI，通过构造器注入很容易写出集成测试
