在 Spring Boot 不同版本中，默认使用的动态代理方式发生了变化，主要原因是为了解决实际开发中的兼容性、一致性和维护性问题。

---

Spring Boot 各版本默认动态代理方式

- Spring Boot 1.x 及更早版本  
  默认使用 JDK 动态代理。  
  - 如果目标类实现了接口 → 使用 JDK 动态代理。  
  - 如果目标类未实现接口 → 使用 CGLIB 代理。  

- Spring Boot 2.x 及以后版本（包括当前主流的 2.x 和 3.x）  
  默认统一使用 CGLIB 动态代理，无论目标类是否实现接口。  
  - 可通过配置 `spring.aop.proxy-target-class=false` 切回“有接口则用 JDK”的旧策略。

---

为什么 Spring Boot 2.x 要改变默认代理方式？

主要有以下几方面原因：

1. 避免类型转换异常（ClassCastException）  
   JDK 动态代理生成的代理对象只能赋值给接口类型。若开发者错误地将代理对象赋值给目标类的实现类，会抛出 `ClassCastException`。CGLIB 生成的是目标类的子类，可安全赋值给父类引用，减少此类运行时错误 。

2. 统一行为，提升可预测性  
   开发者无需关心“这个类有没有接口”，默认统一用 CGLIB，行为更一致，降低认知负担 。

3. 解决方法自调用导致的 AOP 失效问题  
   使用 JDK 代理时，若目标类内部通过 `this` 调用其他方法（如 `@Transactional` 方法），调用会绕过代理，导致 AOP 无效。CGLIB 代理作为子类，能更好地处理这类场景（仍需配合 `@EnableAspectJAutoProxy(proxyTargetClass = true)`）。

4. 性能差距已不显著  
   现代 JVM 对反射优化显著，JDK 与 CGLIB 的性能差异在多数业务场景中可忽略，因此不再以“性能优先”为由坚持 JDK 代理 。

5. 减少对“面向接口编程”的强制依赖  
   Spring Boot 鼓励简化开发，不强制要求每个 Bean 都必须有接口。CGLIB 可直接代理普通类，更契合现代 Spring 开发风格 。

---

如何配置代理方式？

在 `application.properties` 或 `application.yml` 中设置：

```properties
强制使用 CGLIB（Spring Boot 2.x+ 默认值）
spring.aop.proxy-target-class=true

有接口时使用 JDK 动态代理（兼容 Spring 1.x 行为）
spring.aop.proxy-target-class=false
```

> ⚠️ 注意：若目标类未实现任何接口，即使设置为 `false`，Spring 仍会自动降级使用 CGLIB，因为 JDK 代理无法工作 。

---

总结

| 版本 | 默认代理 | 关键原因 |
|------|----------|----------|
| Spring Boot 1.x | JDK（有接口时） | 遵循传统 Spring 行为，优先使用标准库 |
| Spring Boot 2.x+ | CGLIB（统一） | 提升稳定性、一致性，减少开发陷阱，适应现代开发范式 |

这一变更体现了 Spring Boot “约定优于配置” 的设计理念，让开发者更专注于业务逻辑，而非底层代理机制。