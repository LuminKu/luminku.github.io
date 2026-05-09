在 Spring 官方文档和主流实践中，Starter 配置的方式主要围绕“约定优于配置”和“自动装配”机制展开。根据权威公开资料（截至 2026 年），Spring 官方 Starter 的配置方式可归纳如下：

1. 依赖引入（POM 配置）
- 核心方式：通过在 `pom.xml`（Maven）或 `build.gradle`（Gradle）中添加 Starter 依赖，即可自动引入相关功能模块及其依赖。
- 命名规范：
  - 官方 Starter：`spring-boot-starter-{name}`，如 `spring-boot-starter-web`、`spring-boot-starter-data-jpa`。
  - 第三方集成：`{name}-spring-boot-starter`，如 `mybatis-spring-boot-starter`。
- 作用：Starter 本身通常不包含业务代码，而是聚合了自动配置、默认依赖和必要库，实现“开箱即用”。

2. 属性配置（application.properties / application.yml）
- 覆盖默认行为：通过配置文件自定义 Starter 的默认参数。
- 示例：
  - Web 服务端口：`server.port=9090`
  - 数据源配置：`spring.datasource.url=jdbc:mysql://localhost:3306/mydb`
- 机制：Spring Boot 使用 `@ConfigurationProperties` 注解将配置文件中的属性绑定到 Java 对象，实现类型安全的配置注入。

3. 自动配置（Auto-Configuration）
- 核心机制：Starter 依赖 `spring-boot-autoconfigure` 模块，该模块包含一系列 `@Configuration` 类，根据类路径、Bean 存在性等条件动态装配组件。
- 关键注解：
  - `@ConditionalOnClass`：类路径存在指定类时生效。
  - `@ConditionalOnMissingBean`：容器中不存在指定 Bean 时才创建默认实例。
  - `@ConditionalOnProperty`：根据配置属性值决定是否启用配置。
- 加载方式：
  - Spring Boot 2.6 及之前：通过 `META-INF/spring.factories` 文件声明自动配置类。
  - Spring Boot 2.7+：改用 `META-INF/spring/org.springframework.boot.autoconfigure.AutoConfiguration.imports` 文件，提升性能并简化结构。

4. 自定义配置（Java Config）
- 深度定制：若需完全控制配置，可编写自己的 `@Configuration` 类，或实现 `WebMvcConfigurer`、`DataSourceConfigurer` 等接口。
- 注意：若使用 `@EnableWebMvc` 等注解，会禁用 Spring Boot 的默认自动配置，需手动配置所有相关组件。

---

总结
Spring 官方 Starter 的配置方式以依赖引入 + 属性配置 + 自动装配为主，辅以Java 配置进行定制。开发者通常只需：
1. 添加 Starter 依赖；
2. 在 `application.properties` 或 `application.yml` 中按需覆盖属性；
3. 无需手动编写大量 XML 或 Java 配置，即可快速集成功能。

如需进一步了解具体 Starter 的配置项，可查阅 [Spring Boot 官方文档 - Production-ready Features](https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/production-ready)。