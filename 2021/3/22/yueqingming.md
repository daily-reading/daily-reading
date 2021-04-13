[Modern Best Practices for Testing in Java](https://phauer.com/2019/modern-best-practices-testing-java/#tldr)

* Write small and specific tests
* write  self-contained  tests 
* write dump tests
* KISS > DRY
* Test close to production


## General:
**Given,When,Then(单测的组成)**

输入 --> 处理 --> 输出

```Java
// Do
@Test
public void findProduct() {
    insertIntoDatabase(new Product(100, "Smartphone"));

    Product product = dao.findProduct(100);

    assertThat(product.getName()).isEqualTo("Smartphone");
}
```

**Use the Prefixes “actual.” and “expected.”（命名）**
```Java
// Do
ProductDTO actualProduct = requestProduct(1);

ProductDTO expectedProduct = new ProductDTO("1", List.of(State.ACTIVE, State.REJECTED))
assertThat(actualProduct).isEqualTo(expectedProduct); // nice and clear.
```

**Use Fixed Data Instead of Randomized Data**
随机函数意味着测试不具有重复性，难以复现某次测试中的问题。

## Write small and specific tests

将一些重复的或者过于细节的代码提取成为一个函数，保持单元测试的主体方法的简洁性，以便能够在看到函数的第一时间了解具体的步骤。

### Don't Overuse Variables
过多的使用变量会导致测试代码变长，导致不直观。
```Java
// Don't
@Test
public void variables() throws Exception {
    String relevantCategory = "Office";
    String id1 = "4243";
    String id2 = "1123";
    String id3 = "9213";
    String irrelevantCategory = "Hardware";
    insertIntoDatabase(
            createProductWithCategory(id1, relevantCategory),
            createProductWithCategory(id2, relevantCategory),
            createProductWithCategory(id3, irrelevantCategory)
    );

    String responseJson = requestProductsByCategory(relevantCategory);

    assertThat(toDTOs(responseJson))
            .extracting(ProductDTO::getId)
            .containsOnly(id1, id2);
}
```
### KISS > DRY
``` Java
// Do
@Test
public void variables() throws Exception {
    insertIntoDatabase(
            createProductWithCategory("4243", "Office"),
            createProductWithCategory("1123", "Office"),
            createProductWithCategory("9213", "Hardware")
    );

    String responseJson = requestProductsByCategory("Office");

    assertThat(toDTOs(responseJson))
            .extracting(ProductDTO::getId)
            .containsOnly("4243", "1123");
}
```

### Assert Only What You Want to Test
不要重复的测试，知道你要验证什么。

## Self-contained Tests

### Don’t Hide the Relevant Parameters (in Helper Functions) 
``` Java
// Don't
insertIntoDatabase(createProduct());
List<ProductDTO> actualProducts = requestProductsByCategory();
assertThat(actualProducts).containsOnly(new ProductDTO("1", "Office"));
// Do
insertIntoDatabase(createProduct("1", "Office"));
List<ProductDTO> actualProducts = requestProductsByCategory("Office");
assertThat(actualProducts).containsOnly(new ProductDTO("1", "Office"));
```

### Insert Test Data Right In The Test Method 

为了仅通过观察测试代码理解是在测试什么，建议通过 helper functions 来构建参数，而不是@before 之类的方式

### Favor Composition Over Inheritance
编写程序也遵循同样的规则，组合而不是继承。
``` Java
// Do
public class MyTest {
    // composition instead of inheritance
    private JdbcTemplate template;
    private MockWebServer taxService;

    @BeforeAll
    public void setupDatabaseSchemaAndMockWebServer() throws IOException {
        this.template = new DatabaseFixture().startDatabaseAndCreateSchema();
        this.taxService = new MockWebServer();
        taxService.start();
    }
}

// In a different File
public class DatabaseFixture {
    public JdbcTemplate startDatabaseAndCreateSchema() throws IOException {
        PostgreSQLContainer db = new PostgreSQLContainer("postgres:11.2-alpine");
        db.start();
        DataSource dataSource = DataSourceBuilder.create()
                .driverClassName("org.postgresql.Driver")
                .username(db.getUsername())
                .password(db.getPassword())
                .url(db.getJdbcUrl())
                .build();
        JdbcTemplate template = new JdbcTemplate(dataSource);
        SchemaCreator.createSchema(template);
        return template;
    }
}
```

## Dumb Tests Are Great: Compare the Output with Hard-Coded Values

### Don’t Reuse Production Code 

不能即是裁判又是选手

### Don't rewrite Production Logic
这个和上一条有一些对应，虽然不能复用 Production Code 但是也不能重写 Production Logic，专注于 Input 和 Output

### Don't Write Too Much Logic

## Test Close To The Reality
### Focus on Testing A Complete Vertical Slide
在目前的分层架构下，这种集成的垂直测试，似乎相对于单元测试来说更适合也更直观？
![e6b2fe7fb55a4f298b66da6bc9105ea1.png](evernotecid://7F01B206-A616-4EA4-AABF-0C5FA9B75355/appyinxiangcom/26825047/ENResource/p479)

### Don't Use In-Memory Databases For Tests
内存数据库和实际的数据库存在许多不同，使用内存数据库的测试不能担保生产环境没有问题，那么就没有测试的意义。

![abc75f7203b946e986389b9f29b1a795.png](evernotecid://7F01B206-A616-4EA4-AABF-0C5FA9B75355/appyinxiangcom/26825047/ENResource/p480)

## Java/JVM
Use -noverify -XX:TieredStopAtLevel=1(减少测试启动时间)

### Use AssertJ

### Use Junit5

