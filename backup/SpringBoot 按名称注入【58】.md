# [SpringBoot 按名称注入](https://blog.csdn.net/tanglihai/article/details/104495309)
[1](https://blog.csdn.net/tanglihai/article/details/104495309)
[2](https://blog.51cto.com/u_16213302/12133410)
[3](https://juejin.cn/post/7493866403407134746)
在 SpringBoot 中，默认的依赖注入是按类型进行的，但当容器中存在多个相同类型的 Bean 时，就需要按名称来精确指定要注入的 Bean。常用方式有 @Autowired + @Qualifier 和 @Resource。

示例：使用 @Qualifier 按名称注入
```java
@Service("studentServiceImpl1")
public class StudentServiceImpl1 implements StudentService {
  public String getName() { return "Service 1"; }
}

@Service("studentServiceImpl2")
  public class StudentServiceImpl2 implements StudentService {
  public String getName() { return "Service 2"; }
}

@RestController
public class TestController {

@Autowired
@Qualifier("studentServiceImpl1") // 指定 Bean 名称
private StudentService studentService;

@GetMapping("/test")
public String test() {
    return studentService.getName();
 }
}
```

运行后访问 /test 会返回 Service 1，因为通过 @Qualifier 明确了注入的 Bean 名称。

方式一：@Autowired + @Qualifier

@Autowired 默认按类型注入。

搭配 @Qualifier("beanName") 可指定 Bean 名称，解决多个实现类冲突问题。

常用于接口有多实现时的精确注入。

方式二：@Resource(name="beanName")

```java
@Resource(name = "studentServiceImpl2")
private StudentService studentService;
```

来自 JSR-250 规范，默认按名称匹配，找不到时再按类型匹配。

不依赖 Spring 特有注解，兼容性好。

注意事项

Bean 名称来源：默认是类名首字母小写，也可在 @Component/@Service 中显式指定。

冲突解决：当存在多个同类型 Bean 且未指定名称时，会抛出 NoUniqueBeanDefinitionException。

推荐做法： 如果依赖唯一实现类，用构造器注入即可。 多实现类时使用 @Qualifier 或 @Resource 明确指定。

这种按名称注入方式在多实现、多模块项目中非常实用，可以有效避免 Bean 冲突并提升代码可维护性
[1](https://blog.csdn.net/tanglihai/article/details/104495309)
[3](https://juejin.cn/post/7493866403407134746)
。