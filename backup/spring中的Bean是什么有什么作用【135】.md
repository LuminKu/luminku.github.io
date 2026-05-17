在 Spring 框架中，Bean 是由 Spring IoC（控制反转）容器 管理的核心对象，是构成应用程序的基本构建块。

---

什么是 Spring 中的 Bean？

- 官方定义：在 Spring 中，构成应用程序主干并由 Spring IoC 容器管理的对象称为 Bean。Bean 是一个由容器实例化、组装和管理的对象 。
- 本质：Bean 本质上是 Java 类的实例（如 POJO），但其生命周期（创建、依赖注入、销毁等）由 Spring 容器负责，而非开发者手动管理 。
- 常见类型：包括 Service、DAO、Controller、Repository、配置类等 。

---

Bean 的主要作用

- 解耦对象创建与业务逻辑：将对象的创建、依赖关系管理交给 Spring 容器，业务代码无需关心对象如何实例化，提高可维护性和可测试性 。
- 支持依赖注入（DI）：Spring 自动将 Bean 之间的依赖关系注入，避免硬编码，实现松耦合 。
- 统一管理生命周期：容器负责 Bean 的初始化、使用和销毁，尤其对单例 Bean 可精确控制其完整生命周期 。
- 支持多种作用域：可根据应用场景选择不同的 Bean 实例化策略（如单例、原型、请求级等）。

---

Bean 的常见作用域（Scope）

Spring 支持以下作用域，决定 Bean 实例的创建与存活范围：

- `singleton`（默认）：整个 Spring 容器中仅有一个实例，所有请求共享该实例 。
- `prototype`：每次获取 Bean 时都创建一个新实例，适用于有状态对象 。
- `request`（Web 应用）：每个 HTTP 请求创建一个新实例，请求结束后销毁 。
- `session`（Web 应用）：同一个 HTTP Session 共享一个实例，不同 Session 独立 。
- `application`（Web 应用）：整个 Web 应用共享一个实例（ServletContext 级别）。
- `websocket`（Web 应用）：同一个 WebSocket 会话中共享一个实例 。

> ⚠️ 注意：`request`、`session`、`application` 和 `websocket` 仅在基于 Web 的 Spring 应用（如使用 `WebApplicationContext`）中有效 。

---

如何定义 Bean？

- XML 配置：通过 `<bean id="..." class="..." scope="...">` 定义 。
- 注解配置：
  - 使用 `@Component` 及其派生注解（如 `@Service`、`@Repository`、`@Controller`）标记类 。
  - 使用 `@Scope("prototype")` 指定作用域 。
- Java 配置类：使用 `@Configuration` + `@Bean` 方法定义 。

---

总结

Spring Bean 是 Spring IoC 容器管理的对象，核心作用是实现控制反转与依赖注入，从而提升代码的可维护性、可测试性和灵活性。合理选择作用域能有效管理对象的生命周期与状态，尤其在 Web 应用中至关重要 。