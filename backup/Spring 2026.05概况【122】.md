Spring 是一个开源的 Java 企业级应用开发框架，由 Rod Johnson 于 2003 年发起，旨在简化 Java EE（现 Jakarta EE）应用的开发复杂性，提升代码的可测试性、松耦合性和开发效率。

---

核心特点

- 轻量级：完整框架仅需约 1MB 的 JAR 文件，运行开销小。
- 非侵入式：应用对象无需依赖 Spring 特定类，保持 POJO（普通 Java 对象）特性。
- 控制反转（IoC）：对象的创建与依赖管理交由 Spring 容器负责，而非代码中主动 `new`。
- 依赖注入（DI）：IoC 的具体实现方式，通过构造器、Setter 或字段自动注入依赖。
- 面向切面编程（AOP）：支持将横切关注点（如日志、事务、安全）与业务逻辑分离。
- 模块化设计：可按需选择使用核心容器、Web、数据访问、AOP 等模块。
- 一站式解决方案：覆盖表现层、业务层、持久层，支持与主流框架（如 MyBatis、Hibernate、Struts）无缝集成。

---

主要模块组成

1. Core Container（核心容器）
   - `spring-core`：基础工具类
   - `spring-beans`：Bean 管理与配置
   - `spring-context`：应用上下文，支持国际化、事件等
   - `spring-expression`：SpEL 表达式语言

2. Data Access/Integration（数据访问与集成）
   - `spring-jdbc`：简化 JDBC 操作
   - `spring-orm`：集成 Hibernate、JPA 等 ORM 框架
   - `spring-tx`：声明式事务管理

3. Web（Web 支持）
   - `spring-web`：基础 Web 集成
   - `spring-webmvc`：基于 Servlet 的 MVC 实现
   - `spring-webflux`：响应式 Web 框架

4. AOP & Instrumentation（面向切面与检测）
   - `spring-aop`：AOP 功能
   - `spring-aspects`：与 AspectJ 集成

5. Test（测试支持）：集成 JUnit、TestNG，支持注解驱动测试

---

版本信息（截至 2026 年 5 月）

- 最新稳定版：Spring Framework 6.2.5（2024 年 11 月发布）
- 当前主流版本：6.x 系列，要求 Java 17+，基于 Jakarta EE 9+（如 Servlet 5.0+）
- 未来版本：Spring Framework 7.0 已于 2025 年 11 月发布，维护版 7.0.4 于 2026 年 2 月发布，支持 JDK 25 LTS 与 Jakarta EE 11

---

官方资源

- 官网：[https://spring.io](https://spring.io)
- 文档与示例：[Spring Framework Reference Documentation](https://docs.spring.io/spring-framework/docs/current/reference/html/)

> Spring 不仅是一个框架，更是一个生态体系，包含 Spring Boot、Spring Cloud、Spring Security 等众多项目，广泛应用于企业级 Java 开发中。