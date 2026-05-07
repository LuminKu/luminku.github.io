在 Spring 官方文档和主流实践中，Spring Boot Starter 的配置方式主要围绕“约定优于配置”原则，通过依赖引入 + 自动配置 + 属性覆盖实现。以下是核心配置方式：

---

1. 依赖引入（Maven/Gradle）
- 官方 Starter：命名格式为 `spring-boot-starter-{name}`，如：
  - `spring-boot-starter-web`（Web 开发）
  - `spring-boot-starter-data-jpa`（JPA 数据访问）
  - `spring-boot-starter-security`（安全控制）
- 第三方集成 Starter：命名格式为 `{name}-spring-boot-starter`，如：
  - `mybatis-spring-boot-starter`
- 引入后，自动加载相关依赖和默认配置。

> 示例（Maven）：
> ```xml
> <dependency>
>     <groupId>org.springframework.boot</groupId>
>     <artifactId>spring-boot-starter-web</artifactId>
> </dependency>
> ```

---

2. 自动配置（Auto-Configuration）
- Starter 内置 `@Configuration` 类，通过 `@Conditional` 注解按条件生效（如类路径存在某类、缺少特定 Bean 等）。
- 配置类通常位于 `META-INF/spring/org.springframework.boot.autoconfigure.AutoConfiguration.imports`（Spring Boot 2.7+）或 `spring.factories`（旧版）中声明。
- 示例：`WebMvcAutoConfiguration` 在检测到 `DispatcherServlet` 时自动配置 Spring MVC。

---

3. 属性覆盖（外部化配置）
通过以下方式覆盖默认配置（优先级从高到低）：
- 命令行参数：`--server.port=9000`
- 系统环境变量：`SERVER_PORT=9000`
- 配置文件：
  - `application.properties`（项目根目录或 `config/` 子目录）
  - `application.yml`
- 固化代码配置：如 `@Bean` 定义或 `@ConfigurationProperties`

> 示例（`application.properties`）：
> ```properties
> server.port=9000
> logging.level.com.example=DEBUG
> ```

---

4. 自定义 Starter（可选）
若需封装内部组件，可创建自定义 Starter：
1. 自动配置模块：包含 `@Configuration` 类、`@ConfigurationProperties` 和条件注解。
2. Starter 模块：仅依赖自动配置模块，无业务代码。
3. 发布后，其他项目通过依赖引入即可使用 。

---

关键机制总结
- 开箱即用：依赖 + 自动配置 → 功能立即可用。
- 灵活覆盖：通过外部配置调整行为，无需修改代码。
- 条件装配：按环境智能加载组件，避免冲突。

更多细节可参考 [Spring Boot 官方文档](https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/features.developing-auto-configuration)。