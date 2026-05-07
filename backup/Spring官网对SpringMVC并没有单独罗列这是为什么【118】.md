Spring 官网没有将 Spring MVC 作为独立项目单独罗列，主要原因如下：

- Spring MVC 已整合进 Spring Framework 的核心模块：自 Spring 2.0 起，Spring MVC 作为 Web 层框架，被集成在 `spring-webmvc` 模块中，属于 Spring Framework 的一部分，而非独立项目 。
- 官方文档统一归类于 Spring Framework 的 Web 部分：在 [Spring 官方文档](https://docs.spring.io/spring-framework/reference/web/webmvc.html) 中，Spring MVC 的内容被包含在 “Web” → “Spring Web MVC” 章节下，未单独列出入口 。
- Spring Boot 推动了技术栈的融合：随着 Spring Boot 的普及，开发者通常通过 `spring-boot-starter-web` 依赖引入 Spring MVC，进一步弱化了其作为独立组件的可见性 。

因此，Spring 官网将 Spring MVC 视为 Spring Framework 的核心 Web 功能，而非独立项目，故未在项目列表中单独展示。如需查阅相关文档，可访问：

- [Spring Framework 官方文档 - Web MVC](https://docs.spring.io/spring-framework/reference/web/webmvc.html)
- [Spring 官网主页面 → Projects → Spring Framework → Reference Documentation](https://spring.io/projects/spring-framework) 