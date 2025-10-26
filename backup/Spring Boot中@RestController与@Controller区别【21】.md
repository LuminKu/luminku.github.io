这是一个关于 Spring Boot 中两个核心注解的经典问题。在 Spring Boot 3.5（它基于 Spring Framework 6）中，`@RestController` 和 `@Controller` 的区别与之前版本的核心思想一致，但理解它们对于构建现代 Web 应用至关重要。

### 核心区别一句话总结

**`@Controller` 用于传统的 Web 视图应用（返回 HTML 页面），而 `@RestController` 用于构建 RESTful Web 服务（返回 JSON/XML 数据）。**

`@RestController` 本质上是 `@Controller` 和 `@ResponseBody` 两个注解的合体，它为所有方法默认添加了 `@ResponseBody` 语义。

---

### 详细对比

| 特性 | `@Controller` | `@RestController` |
| :--- | :--- | :--- |
| **本质** | 一个特殊的 `@Component`，用于标记一个类作为 Web 请求的处理器（Handler）。 | **`@Controller` + `@ResponseBody`**。它是 `@Controller` 的一个特化/组合注解。 |
| **返回值的含义** | 返回值通常是一个**视图名称**（String 类型），由视图解析器（View Resolver）解析为具体的视图（如 JSP, Thymeleaf, FreeMarker 模板）。 | 返回值直接通过 **HTTP 消息转换器（HttpMessageConverter）** 序列化后写入 **HTTP 响应体（ResponseBody）**。 |
| **典型应用场景** | 传统的服务端渲染（SSR）应用。用户请求一个页面，控制器处理逻辑，然后返回一个填充了数据的 HTML 页面。 | **RESTful API** 或 **Web服务**。前端（浏览器、手机App等）通过 AJAX 调用接口，后端返回纯数据（通常是 JSON），由前端负责渲染。 |
| **是否需要配合其他注解** | 如果需要返回数据而不是视图，**需要**在方法上额外添加 **`@ResponseBody`** 注解。 | **不需要**。它的所有方法都自动具有 `@ResponseBody` 的效果。 |
| **HTTP 响应 Content-Type** | 默认是 `text/html`，因为返回的是 HTML 页面。 | 默认是 `application/json`（如果返回的是对象），因为返回的是 JSON 数据。 |

---

### 代码示例

#### 1. 使用 `@Controller`（返回视图）
```java
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.GetMapping;

@Controller // 表示这是一个控制器，返回值通常对应一个视图文件
public class WelcomeController {

    @GetMapping("/welcome")
    public String welcome(Model model) {
        model.addAttribute("message", "Hello, Spring Boot!");
        // 返回一个视图名称，视图解析器会去找 /templates/welcome.html 文件
        return "welcome"; 
    }
}
```
对应的 `src/main/resources/templates/welcome.html` (Thymeleaf 模板示例)：
```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head>
    <title>Welcome</title>
</head>
<body>
    <h1 th:text="${message}">Default Message</h1>
</body>
</html>
```
**访问 `http://localhost:8080/welcome` 会得到一个渲染好的 HTML 页面。**

#### 2. 使用 `@Controller`（返回数据，需配合 `@ResponseBody`）
```java
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.ResponseBody;

@Controller
public class ApiController {

    @GetMapping("/user")
    @ResponseBody // 关键注解：告诉Spring返回值直接写入响应体，不进行视图解析
    public User getUser() {
        return new User("Alice", 30);
    }
}
```

#### 3. 使用 `@RestController`（返回数据，推荐方式）
```java
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController // 等价于 @Controller + @ResponseBody
public class UserApiController {

    @GetMapping("/api/user")
    // 无需 @ResponseBody，自动将 User 对象转换为 JSON
    public User getUser() {
        return new User("Bob", 25);
    }
}

// 简单的User类
class User {
    private String name;
    private int age;
    // 构造方法、Getter和Setter省略...
}
```
**访问 `http://localhost:8080/api/user` 会得到一段 JSON 数据：`{"name":"Bob","age":25}`。**

---

### 历史背景与总结

*   **Spring 3.0** 引入了 `@Controller` 来替代基于 XML 的控制器配置。
*   **Spring 4.0** 引入了 `@RestController`。随着前后端分离架构和单页应用（SPA）的流行，开发人员发现他们的大部分控制器方法都需要 `@ResponseBody`。为了减少重复注解，Spring 团队创建了 `@RestController` 这个便利的复合注解。

**如何选择？**

*   如果你的项目是**传统的服务端渲染 Web 应用**（比如使用 Thymeleaf、FreeMarker、JSP 来生成页面），使用 **`@Controller`**。
*   如果你的项目主要提供 **RESTful API** 为前端（如 React, Vue, Angular）或移动端提供数据接口，使用 **`@RestController`**。
*   在同一个项目中，你甚至可以**混合使用**。例如，一个 `@Controller` 负责处理页面跳转，而另一个 `@RestController` 负责提供该页面所需的 Ajax API 数据。