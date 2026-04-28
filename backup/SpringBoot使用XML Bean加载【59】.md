在Spring Boot中，虽然推荐使用Java配置类（`@Configuration`），但仍然可以通过`@ImportResource`注解来加载传统的`applicationContext.xml`文件定义Bean。以下是完整示例：

### 1. 创建XML配置文件

在`src/main/resources`下创建`applicationContext.xml`：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd">

    <!-- 定义一个简单的Bean -->
    <bean id="userService" class="com.example.demo.service.UserService">
        <property name="username" value="admin"/>
        <property name="timeout" value="30"/>
    </bean>

    <!-- 定义一个带构造参数的Bean -->
    <bean id="orderService" class="com.example.demo.service.OrderService">
        <constructor-arg name="orderPrefix" value="ORD-"/>
        <constructor-arg name="maxItems" value="100"/>
    </bean>

</beans>
```

### 2. 创建对应的Java类

**UserService.java:**
```java
package com.example.demo.service;

public class UserService {
    private String username;
    private int timeout;

    // 必须提供setter方法用于XML注入
    public void setUsername(String username) {
        this.username = username;
    }

    public void setTimeout(int timeout) {
        this.timeout = timeout;
    }

    public void printConfig() {
        System.out.println("UserService - Username: " + username + ", Timeout: " + timeout);
    }
}
```

**OrderService.java:**
```java
package com.example.demo.service;

public class OrderService {
    private String orderPrefix;
    private int maxItems;

    // 构造方法注入
    public OrderService(String orderPrefix, int maxItems) {
        this.orderPrefix = orderPrefix;
        this.maxItems = maxItems;
    }

    public void printConfig() {
        System.out.println("OrderService - Prefix: " + orderPrefix + ", Max Items: " + maxItems);
    }
}
```

### 3. 在启动类中加载XML配置

**DemoApplication.java:**
```java
package com.example.demo;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.context.annotation.ImportResource;

@SpringBootApplication
// 关键：加载XML配置文件
@ImportResource("classpath:applicationContext.xml")
public class DemoApplication {
    public static void main(String[] args) {
        SpringApplication.run(DemoApplication.class, args);
    }
}
```

### 4. 注入并使用Bean

**TestController.java:**
```java
package com.example.demo.controller;

import com.example.demo.service.OrderService;
import com.example.demo.service.UserService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class TestController {

    @Autowired
    private UserService userService;

    @Autowired
    private OrderService orderService;

    @GetMapping("/test")
    public String testBeans() {
        userService.printConfig();
        orderService.printConfig();
        return "Check console output!";
    }
}
```

### 5. 项目结构

```
src
└── main
    ├── java
    │   └── com
    │       └── example
    │           └── demo
    │               ├── DemoApplication.java
    │               ├── controller
    │               │   └── TestController.java
    │               └── service
    │                   ├── UserService.java
    │                   └── OrderService.java
    └── resources
        └── applicationContext.xml
```

### 6. 访问测试

启动应用后访问：`http://localhost:8080/test`

控制台输出：
```
UserService - Username: admin, Timeout: 30
OrderService - Prefix: ORD-, Max Items: 100
```

### 关键点总结

1.  **`@ImportResource`**  ：在配置类或启动类上添加此注解来加载XML文件
2. **XML文件位置**：默认从`classpath:`根路径加载
3. **属性注入**：XML中的`<property>`需要对应Java类的setter方法
4. **构造注入**：XML中的`<constructor-arg>`需要对应构造方法参数
5. **混合使用**：可以与Java Config（`@Bean`）方式共存

这种方式适合在迁移旧项目或需要复用现有XML配置时使用，但新项目建议优先使用Java配置方式。

---
---

在测试类中直接使用`@ImportResource`不生效的原因是：**Spring Boot测试默认加载的是主应用类的上下文配置，测试类上的`@ImportResource`不会自动生效**。

以下是几种解决方案：

### 方案1：将`@ImportResource`移到主应用类（推荐）

**DemoApplication.java**（主启动类添加注解）
```java
@SpringBootApplication
@ImportResource("classpath:applicationContext.xml") // 移到此处
public class DemoApplication {
    public static void main(String[] args) {
        SpringApplication.run(DemoApplication.class, args);
    }
}
```

**K05XmlTests.java**（测试类保持不变）
```java
@SpringBootTest
public class K05XmlTests {
    @Autowired
    private UserService userService;
    
    @Autowired
    private OrderService orderService;

    @Test
    void test() {
        userService.printConfig();  // 正常输出
        orderService.printConfig(); // 正常输出
    }
}
```

### 方案2：测试类指定加载配置

如果不想让主应用类依赖XML，可以在测试中单独加载：

```java
@ImportResource("classpath:applicationContext.xml")
@SpringBootTest(classes = {K05XmlTests.Config.class}) // 指定配置类
public class K05XmlTests {

    @Autowired
    private UserService userService;
    
    @Autowired
    private OrderService orderService;

    @Test
    void test() {
        userService.printConfig();
        orderService.printConfig();
    }

    @Configuration // 空配置类，仅用于加载XML
    static class Config {
    }
}
```

### 方案3：使用`@ContextConfiguration`显式指定

```java
@ExtendWith(SpringExtension.class)
@ContextConfiguration(locations = "classpath:applicationContext.xml")
public class K05XmlTests {

    @Autowired
    private UserService userService;
    
    @Autowired
    private OrderService orderService;

    @Test
    void test() {
        userService.printConfig();
        orderService.printConfig();
    }
}
```

### 方案4：组合注解方式

```java
@SpringBootTest
@ImportResource("classpath:applicationContext.xml")
public class K05XmlTests {

    @Autowired
    private UserService userService;
    
    @Autowired
    private OrderService orderService;

    @TestConfiguration // 确保上下文加载
    static class TestConfig {
    }

    @Test
    void test() {
        userService.printConfig();
        orderService.printConfig();
    }
}
```

### 关键点总结

| 方案 | 适用场景 | 优点 | 缺点 |
|------|----------|------|------|
| 方案1 | 整个项目都使用XML配置 | 简单，一次配置全局生效 | 主应用依赖XML |
| 方案2 | 仅测试需要XML配置 | 隔离性好，不影响主应用 | 配置稍繁琐 |
| 方案3 | 纯Spring测试，不依赖Boot | 传统Spring测试方式 | 不推荐用于Spring Boot |
| 方案4 | 测试需要额外Bean时 | 灵活，可扩展 | 需要理解内部机制 |

**推荐做法**：如果XML配置是整个项目的一部分，使用** 方案1**；如果XML只用于特定测试，使用**方案2 **。

**调试技巧**：在测试中添加以下代码检查加载的Bean：
```java
@Autowired
private ApplicationContext context;

@Test
void debugBeans() {
    System.out.println(Arrays.toString(context.getBeanDefinitionNames()));
}
```

