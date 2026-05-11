在 JDK 17 中使用 CGLIB，需注意以下关键点：

- CGLIB 依赖 ASM 字节码操作库，而 JDK 9+ 引入了模块化系统（JPMS），限制了对 `java.base` 模块内部类的访问。
- CGLIB 3.3.0 及以上版本 才能较好兼容 JDK 17 。
- 若使用 CGLIB 3.3.0+，仍可能因模块访问问题抛出 `InaccessibleObjectException`，需通过 JVM 参数 `--add-opens` 解决 。

---

Maven 配置步骤

1. 添加 CGLIB 依赖（推荐使用 `3.3.0` 或更高版本）：

```xml
<dependency>
    <groupId>cglib</groupId>
    <artifactId>cglib</artifactId>
    <version>3.3.0</version>
</dependency>
```

2. 可选：显式指定 ASM 版本（避免版本冲突，推荐 `7.1+`）：

```xml
<dependency>
    <groupId>org.ow2.asm</groupId>
    <artifactId>asm</artifactId>
    <version>9.4</version> <!-- 或 7.1，根据项目需求 -->
</dependency>
```

> 注：CGLIB 3.3.0+ 已内置适配 ASM 7+，通常无需手动引入，但若出现字节码兼容问题，可显式指定 。

---

运行时配置（解决模块访问问题）

在运行 Java 程序时，添加以下 JVM 参数以开放 `java.base` 模块的内部访问：

```bash
--add-opens java.base/java.lang=ALL-UNNAMED
```

- IntelliJ IDEA 配置方式：  
  `Run → Edit Configurations → VM options` 中添加上述参数 。

- Maven 编译运行时（如使用 `mvn exec:java`）：  
  在 `pom.xml` 的 `exec-maven-plugin` 配置中添加：

```xml
<plugin>
    <groupId>org.codehaus.mojo</groupId>
    <artifactId>exec-maven-plugin</artifactId>
    <configuration>
        <executable>java</executable>
        <arguments>
            <argument>--add-opens</argument>
            <argument>java.base/java.lang=ALL-UNNAMED</argument>
            <argument>-classpath</argument>
            <classpath/>
            <argument>com.example.Main</argument>
        </arguments>
    </configuration>
</plugin>
```

---

注意事项

- 被代理类不能是 `final`，方法也不能是 `final`，否则 CGLIB 会抛出异常 。
- 避免过度使用 CGLIB，动态生成的类会占用 Metaspace，可能导致内存溢出 。
- 若项目使用 Spring，Spring AOP 默认在无接口时自动使用 CGLIB，无需手动配置 。

---

推荐版本组合（JDK 17 环境）

| 组件       | 推荐版本 |
|------------|----------|
| CGLIB  | `3.3.0` 或更高  |
| ASM    | `7.1` ~ `9.4`  |

更多详情可参考 [CGLIB 官方仓库](https://gitcode.com/gh_mirrors/cg/cglib) 。