Jakarta EE 简介

Jakarta EE 是由 Eclipse 基金会 管理的开源企业级 Java 平台，前身为 Java EE（Java Platform, Enterprise Edition），最初由 Sun 公司推出，2009 年 Oracle 收购 Sun 后接管，2017 年移交至 Eclipse 基金会，并于 2018 年正式更名为 Jakarta EE 。

该平台基于 Java SE（Standard Edition）构建，提供一套标准化的 API 和运行时环境，用于开发可移植、健壮、安全的企业级 Web 应用与微服务。其核心特性包括：

- 支持 Servlet、JSP、JPA、EJB、JMS、CDI、JTA、JAX-RS 等关键规范；
- 提供 Web 容器（运行 Servlet/JSP）和 EJB 容器（管理业务逻辑）；
- 支持 事务管理、安全性、分布式通信、数据持久化 等企业级功能；
- 近年重点向 云原生、微服务、轻量化 演进，集成 MicroProfile 规范 。

---

Jakarta EE 与 Java 的区别与联系

> 这里的“Java”通常指 Java SE（标准版），而非整个 Java 生态。

核心关系

- 依赖关系：Jakarta EE 完全依赖 Java SE，所有 Jakarta EE 规范均构建在 Java SE 的核心类库（如 `java.lang`、`java.io`、`java.net`）之上 。
- 定位不同：
  - Java SE：提供语言基础与通用开发能力，适用于桌面应用、命令行工具、简单后端等；
  - Jakarta EE：专注于企业级后端开发，解决高并发、分布式、事务、安全等复杂场景 。

对比总结

| 维度 | Java SE | Jakarta EE |
|------|---------|------------|
| 性质 | 基础开发工具包（含 JDK、JRE、核心类库） | 企业级应用开发规范（仅定义接口） |
| 用途 | 通用 Java 开发（桌面、基础服务等） | 企业级 Web 应用、微服务、分布式系统 |
| 是否需额外容器 | 否，可直接运行 | 是，需部署于支持 Jakarta EE 的应用服务器（如 WildFly、Payara、Tomcat 10+） |
| 典型技术 | `java.util`, `java.io`, 多线程等 | `javax.servlet`（旧） / `jakarta.servlet`（新）、JPA、EJB 等 |
| 版本依赖 | 独立演进（如 JDK 8/11/17/21） | Jakarta EE 9+ 要求 JDK 11+，EE 10+ 要求 JDK 17+  |

关键变化点

- 包名变更：Jakarta EE 9 起，包名从 `javax.*` 改为 `jakarta.*`，例如：
  ```java
  // Java EE 8 及以前
  import javax.servlet.http.HttpServlet;
  
  // Jakarta EE 9 及以后
  import jakarta.servlet.http.HttpServlet; // 
  ```
- 治理模式：从 Oracle 主导转向 Eclipse 基金会社区驱动，更开放、敏捷 。

---

总结

- Java SE 是地基，Jakarta EE 是建在其上的企业级应用规范体系；
- 两者不是替代关系，而是分层协作关系；
- 学习 Jakarta EE 必须先掌握 Java SE；
- 当前主流企业后端开发中，Jakarta EE 与 Spring Boot 等框架并存，前者强调标准化，后者强调灵活性与开箱即用 。