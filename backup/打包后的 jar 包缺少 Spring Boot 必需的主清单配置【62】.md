你在执行`java -jar`命令运行Spring Boot的jar包时，遇到了“没有主清单属性”的错误，核心原因是**打包后的jar包缺少Spring Boot必需的主清单配置**（META-INF/MANIFEST.MF里没有指定程序入口类、Spring Boot启动类等关键属性），导致JVM不知道该从哪个类启动程序。下面我会先解释原因，再给出具体的解决步骤（适配最常用的Maven构建方式）。

### 一、错误原因详解
Spring Boot的可执行jar包和普通jar包不同，它需要在`META-INF/MANIFEST.MF`文件中包含以下核心“主清单属性”：
- `Main-Class`：JVM的入口类（固定为`org.springframework.boot.loader.JarLauncher`）；
- `Start-Class`：你自己的Spring Boot主类（比如`com.example.Springboot01QuickstartApplication`）；
- `Spring-Boot-Version`：Spring Boot版本号。

出现“没有主清单属性”，本质是**你的Maven/Gradle构建配置中缺少Spring Boot打包插件**，导致打包时没有自动生成这些属性。

### 二、解决步骤（Maven项目，99%的Spring Boot项目用Maven）
#### 步骤1：在pom.xml中添加Spring Boot打包插件
这是核心解决步骤，需要在`pom.xml`的`<build><plugins>`中添加`spring-boot-maven-plugin`插件，确保打包时生成正确的清单属性：
```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <!-- 1. 确保继承Spring Boot父依赖（如果没继承，插件配置需更完整，下面会说明） -->
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.7.0</version> <!-- 替换为你的Spring Boot版本 -->
        <relativePath/>
    </parent>

    <groupId>com.example</groupId>
    <artifactId>springboot-01-quickstart</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <name>springboot-01-quickstart</name>

    <!-- 2. 核心：添加Spring Boot打包插件 -->
    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
                <!-- 如果继承了父依赖，版本可省略（自动继承）；否则需手动指定版本 -->
                <version>2.7.0</version> <!-- 与Spring Boot版本一致 -->
                <executions>
                    <execution>
                        <goals>
                            <goal>repackage</goal> <!-- 关键：重新打包为可执行jar -->
                        </goals>
                    </execution>
                </executions>
                <!-- 可选：手动指定启动类（如果插件没自动识别） -->
                <configuration>
                    <mainClass>com.example.Springboot01QuickstartApplication</mainClass>
                </configuration>
            </plugin>
        </plugins>
    </build>
</project>
```

**关键说明**：
- 如果你的项目**没有继承`spring-boot-starter-parent`**，需要在插件中手动指定`spring-boot-starter-parent`的版本，且配置更完整（但推荐优先继承父依赖）；
- `repackage`目标是核心：它会将普通jar包重新打包为Spring Boot可执行jar，自动生成主清单属性。

#### 步骤2：重新打包（清理旧包，生成新包）
打开终端/命令行，进入项目根目录（pom.xml所在目录），执行以下命令：
```bash
# 清理旧的编译/打包文件
mvn clean
# 打包（-DskipTests跳过测试，加快打包速度）
mvn package -DskipTests
```
执行成功后，会在项目的`target`目录下生成新的jar包（就是你要运行的`springboot-01-quickstart-0.0.1-SNAPSHOT.jar`）。

#### 步骤3：重新运行jar包
再次执行`java -jar`命令：
```bash
java -jar target/springboot-01-quickstart-0.0.1-SNAPSHOT.jar
```
此时应该能正常启动Spring Boot应用，不会再提示“没有主清单属性”。

### 三、验证清单属性（可选，确认配置生效）
如果想确认jar包是否包含主清单属性，可以解压jar包查看`META-INF/MANIFEST.MF`文件：
1. 解压jar包（或直接用文本编辑器打开jar包）；
2. 找到`META-INF/MANIFEST.MF`文件，查看是否包含以下内容：
```
Main-Class: org.springframework.boot.loader.JarLauncher
Start-Class: com.example.Springboot01QuickstartApplication
Spring-Boot-Version: 2.7.0
```
只要包含这些属性，说明打包配置正确。

### 四、常见坑点提醒
1. **插件版本不匹配**：插件版本必须和Spring Boot核心依赖版本一致，否则可能打包失败；
2. **启动类没加`@SpringBootApplication`注解**：即使打包配置正确，启动类缺少该注解也会启动失败（但不会报“主清单属性”错误）；
3. **打包成了普通jar而非可执行jar**：执行`mvn package`后，`target`目录可能会生成两个jar包（一个普通jar，一个可执行jar），要运行带`SNAPSHOT.jar`的那个（而非`SNAPSHOT.jar.original`）。

### 总结
1. **核心原因**：缺少Spring Boot打包插件，导致jar包没有生成主清单属性；
2. **解决关键**：在pom.xml中配置`spring-boot-maven-plugin`插件（重点加`repackage`目标）；
3. **操作步骤**：配置插件 → 清理旧包 → 重新打包 → 运行新包。

如果你的项目是Gradle构建的，我可以补充对应的Gradle打包配置。