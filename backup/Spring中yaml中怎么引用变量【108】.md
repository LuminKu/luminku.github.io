在Spring框架中，尤其是在使用YAML配置文件时，经常需要引用外部变量（例如环境变量或配置文件中的变量）。Spring提供了几种方法来引用这些变量，主要依赖于Spring的配置文件加载方式和Spring Boot的特性。以下是一些常用的方法：

1. 使用`@Value`注解

如果你在Spring Boot应用中，可以使用`@Value`注解来注入外部配置的值。例如，如果你有一个环境变量`APP_NAME`，你可以这样使用：

```java
import org.springframework.beans.factory.annotation.Value;
import org.springframework.stereotype.Component;

@Component
public class MyComponent {
    @Value("${app.name}")
    private String appName;

    // 使用appName
}
```

确保你的`application.yml`或`application.properties`文件中定义了相应的属性：

```yaml
app:
  name: MyAppName
```

2. 使用`@ConfigurationProperties`和`@ConstructorBinding`（Spring Boot 2.2+）

对于更复杂的配置对象，可以使用`@ConfigurationProperties`。从Spring Boot 2.2开始，你可以使用`@ConstructorBinding`来创建一个只包含构造函数的bean。

```java
import org.springframework.boot.context.properties.ConfigurationProperties;
import org.springframework.stereotype.Component;

@Component
@ConfigurationProperties(prefix = "app")
public class AppProperties {
    private String name;

    public AppProperties(String name) {
        this.name = name;
    }

    // getters and setters
}
```

在`application.yml`中：

```yaml
app:
  name: MyAppName
```

3. 使用Spring Environment抽象

你也可以通过注入`Environment`对象来访问属性：

```java
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.core.env.Environment;
import org.springframework.stereotype.Component;

@Component
public class MyComponent {
    @Autowired
    private Environment env;

    public void someMethod() {
        String appName = env.getProperty("app.name");
        // 使用appName
    }
}
```

4. 使用命令行参数或环境变量直接引用（适用于所有Spring应用）

对于环境变量和命令行参数，你可以直接在YAML文件中使用`${ENV_VAR_NAME}`语法：

```yaml
app:
  name: ${MY_APP_NAME:defaultName}  如果MY_APP_NAME未设置，则使用defaultName作为默认值。
```

确保你的环境变量已经设置：

```bash
export MY_APP_NAME=MyAppNameFromEnv
```
或者对于Windows：
```cmd
set MY_APP_NAME=MyAppNameFromEnv
```
然后运行你的Spring应用。

5. 使用Spring Cloud Config Server（分布式配置管理）
如果你使用的是Spring Cloud，可以配置一个Config Server来管理配置。这样，你可以从远程服务器获取配置，而不是硬编码在本地文件中。这提供了更好的灵活性和可维护性。例如，你可以从Git仓库或任何HTTP服务器获取配置。

每种方法都有其适用场景，你可以根据具体需求选择最合适的方式。