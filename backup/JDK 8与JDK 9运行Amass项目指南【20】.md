```plain text
如何运行下面项目代码中的类，用jdk8、jdk9分别运行（有什么区别）
amass
    |- src/main/java/cn/xiaxin/Multiple.java
    |- pox.xml
```

> 由于问题中只提供了项目结构，并没有提供具体的代码内容，因此我将假设Multiple.java是一个简单的Java类，并且pom.xml是一个基本的Maven配置文件。
> 为了演示如何运行，我将创建一个简单的Multiple类，它有一个main方法，用于打印一条消息和使用的JDK版本。
> 同时，我将提供一个简单的pom.xml文件。
>
> 注意：由于问题要求用JDK8和JDK9分别运行，我们需要考虑两种环境下的可能差异。在JDK9中，模块系统（JPMS）被引入，但如果我们没有使用模块化，那么代码可能无需修改即可运行。
> 假设我们的代码没有使用任何版本特定的特性，那么运行方式相同。但是，为了演示区别，我们可以在代码中通过系统属性打印java.version。
>
> 步骤：
>
> 1. 创建项目结构
> 2. 编写Multiple.java
> 3. 编写pom.xml
> 4. 使用JDK8编译和运行
> 5. 使用JDK9编译和运行
>
> 由于我们使用Maven，可以通过切换JAVA_HOME来使用不同的JDK版本，然后使用mvn compile exec:java来运行，或者先编译成jar再运行。
>
> 但是，请注意，问题中并没有指定使用Maven exec插件来运行，所以我们也可以使用传统的先编译后运行的方式。
>
> 由于问题中要求运行类，我们可以使用以下两种方式：
>
> 方式一：使用Maven Exec插件（需要配置）
> 方式二：使用java命令直接运行（需要先编译并设置类路径）
>
> 这里我们将使用Maven进行编译，然后使用java命令运行，以便更清楚地看到不同JDK版本下的运行情况。
>
> 假设项目结构如下：
>
> amass
> |-- src/main/java/cn/xiaxin/Multiple.java
> |-- pom.xml
>
> Multiple.java 内容：
>
> text
>
> ```
> package cn.xiaxin;
> 
> public class Multiple {
>     public static void main(String[] args) {
>         System.out.println("Hello, World!");
>         System.out.println("Java Version: " + System.getProperty("java.version"));
>     }
> }
> ```
>
> 
>
> pom.xml 内容：
>
> text
>
> ```
> <project>
>   <modelVersion>4.0.0</modelVersion>
>   <groupId>cn.xiaxin</groupId>
>   <artifactId>amass</artifactId>
>   <version>1.0-SNAPSHOT</version>
> 
>   <properties>
>     <maven.compiler.source>1.8</maven.compiler.source>
>     <maven.compiler.target>1.8</maven.compiler.target>
>     <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
>   </properties>
> 
>   <build>
>     <plugins>
>       <plugin>
>         <groupId>org.apache.maven.plugins</groupId>
>         <artifactId>maven-compiler-plugin</artifactId>
>         <version>3.8.1</version>
>         <configuration>
>           <source>1.8</source>
>           <target>1.8</target>
>         </configuration>
>       </plugin>
>     </plugins>
>   </build>
> </project>
> ```
>
> 
>
> 注意：在pom.xml中，我们指定了编译的源代码和目标字节码版本为1.8，这意味着即使使用JDK9编译，也会生成与JDK8兼容的字节码。
>
> 运行步骤：
>
> 1. 确保设置了JAVA_HOME为JDK8或JDK9的安装路径，并将PATH指向正确的java和javac。
> 2. 使用Maven编译项目：
>    mvn compile
> 3. 运行编译后的类：
>    由于类在target/classes目录下，我们可以使用java命令运行，注意要指定包名。
>
> 对于JDK8：
> java -cp target/classes cn.xiaxin.Multiple
>
> 对于JDK9：
> 同样使用上述命令，因为我们的代码没有使用模块化，所以可以像JDK8一样运行。
>
> 但是，如果我们想要体验JDK9的模块化，那么需要创建module-info.java。但问题中没有要求，所以我们先按照非模块化项目运行。
>
> 区别：
> 在运行的时候，JDK8和JDK9的输出会显示不同的java.version属性值。
>
> 另外，需要注意的是，如果代码中使用了JDK9中废弃的API或者被移除的API，那么在JDK9下运行可能会出现问题。但是我们的简单示例没有这个问题。
>
> 如果使用Maven Exec插件来运行，则需要配置pom.xml中的exec插件。
>
> 下面我们使用Maven Exec插件来运行：
>
> 在pom.xml的build/plugins中添加：
>
> text
>
> ```
> <plugin>
>     <groupId>org.codehaus.mojo</groupId>
>     <artifactId>exec-maven-plugin</artifactId>
>     <version>1.6.0</version>
>     <configuration>
>         <mainClass>cn.xiaxin.Multiple</mainClass>
>     </configuration>
> </plugin>
> ```
>
> 
>
> 然后运行：
> mvn compile exec:java
>
> 这样就会使用当前设置的JDK版本来运行。
>
> 现在，我们分别用JDK8和JDK9来运行这个项目，观察输出中的Java版本。
>
> 由于问题中没有给出具体代码，以上是一种假设的简单情况。如果实际代码中使用了JDK9的新特性，那么需要在JDK9下编译和运行，并且可能无法在JDK8下运行。
>
> 但是根据问题，我们分别用JDK8和JDK9运行，所以如果代码中有版本特定的代码，则需要调整。这里假设代码是兼容JDK8和JDK9的。
>
> 总结步骤：
>
> 1. 设置JAVA_HOME为JDK8路径，编译并运行（使用java命令或Maven exec插件）。
> 2. 设置JAVA_HOME为JDK9路径，编译并运行。
>
> 注意：在切换JDK版本时，最好使用工具如SDKMAN!（在Linux/Mac上）或手动切换JAVA_HOME。
>
> 输出区别：
> 在JDK8下运行，输出的版本字符串可能是1.8.0_XXX。
> 在JDK9下运行，输出的版本字符串可能是9.x.x。
>
> 此外，如果没有其他版本特定代码，程序行为应该一致。
>
> 如果实际项目代码中使用了JDK9的模块化，那么需要额外的步骤（创建module-info.java），并且运行方式也会有所不同（需要使用模块路径而不是类路径）。但是根据问题中的项目结构，并没有模块化，所以我们按传统方式运行。


# 直接使用java.exe命令运行Amass项目

我将详细介绍如何不使用Maven，而是直接使用`java.exe`命令来运行Amass项目中的Multiple类，并分别说明在JDK 8和JDK 9环境下的操作步骤和区别。

## 项目结构回顾
```
amass/
├── src/main/java/cn/xiaxin/Multiple.java
└── pom.xml
```

## 基本步骤（适用于JDK 8和JDK 9）

### 1. 编译Java文件
首先需要将Java源代码编译成字节码文件(.class)。

### 2. 运行编译后的类
使用java命令执行编译后的类。

## JDK 8环境下的操作

### 步骤1：编译Java文件
```bash
# 创建输出目录
mkdir -p target/classes

# 编译Java文件（指定输出目录）
javac -d target/classes src/main/java/cn/xiaxin/Multiple.java
```

### 步骤2：运行编译后的类
```bash
# 运行程序（指定类路径）
java -cp target/classes cn.xiaxin.Multiple
```

## JDK 9环境下的操作

### 步骤1：编译Java文件
```bash
# 创建输出目录
mkdir -p target/classes

# 编译Java文件（与JDK 8命令相同）
javac -d target/classes src/main/java/cn/xiaxin/Multiple.java
```

### 步骤2：运行编译后的类
```bash
# 运行程序（与JDK 8命令相同）
java -cp target/classes cn.xiaxin.Multiple
```

## 处理依赖项

如果Multiple类依赖其他JAR包，需要将这些JAR包添加到类路径中：

```bash
# 假设有lib目录包含依赖的JAR包
java -cp "target/classes:lib/*" cn.xiaxin.Multiple

# Windows系统使用分号分隔
java -cp "target/classes;lib/*" cn.xiaxin.Multiple
```

## JDK 8与JDK 9运行时的区别

虽然基本命令相同，但在JDK 9环境下可能会遇到以下差异：

1. **模块系统相关错误**：
   - 如果代码使用了JDK 9的模块系统特性，可能需要添加模块相关参数
   - 示例：`java --add-modules java.sql -cp target/classes cn.xiaxin.Multiple`

2. **废弃API警告**：
   - JDK 9中某些在JDK 8中可用的API可能已被标记为废弃
   - 编译时可能看到更多警告信息

3. **版本特定代码**：
   - 如果代码中使用了版本特定的API，可能需要条件编译

## 示例：处理版本特定的代码

如果Multiple.java中包含JDK 9特有的代码，可以这样处理：

```java
// Multiple.java
package cn.xiaxin;

public class Multiple {
    public static void main(String[] args) {
        System.out.println("Running with Java version: " + System.getProperty("java.version"));
        
        // 检查Java版本并执行相应代码
        String version = System.getProperty("java.version");
        if (version.startsWith("1.8")) {
            System.out.println("Running on JDK 8");
            // JDK 8特定代码
        } else if (version.startsWith("9")) {
            System.out.println("Running on JDK 9");
            try {
                // 使用反射调用JDK 9特有API，避免编译错误
                Class<?> processHandleClass = Class.forName("java.lang.ProcessHandle");
                Object currentProcess = processHandleClass.getMethod("current").invoke(null);
                long pid = (Long) processHandleClass.getMethod("pid").invoke(currentProcess);
                System.out.println("Process PID: " + pid);
            } catch (Exception e) {
                System.out.println("ProcessHandle API not available");
            }
        }
    }
}
```

## 创建简单的启动脚本

为了方便在不同JDK版本间切换，可以创建启动脚本：

### Linux/Mac脚本 (run.sh)
```bash
#!/bin/bash
JAVA_HOME=${1:-/usr/lib/jvm/java-8-openjdk}  # 默认使用JDK 8
${JAVA_HOME}/bin/javac -d target/classes src/main/java/cn/xiaxin/Multiple.java
${JAVA_HOME}/bin/java -cp target/classes cn.xiaxin.Multiple
```

### Windows脚本 (run.bat)
```batch
@echo off
set JAVA_HOME=%1
if "%JAVA_HOME%"=="" set JAVA_HOME="C:\Program Files\Java\jdk1.8.0_201"
"%JAVA_HOME%\bin\javac" -d target\classes src\main\java\cn\xiaxin\Multiple.java
"%JAVA_HOME%\bin\java" -cp target\classes cn.xiaxin.Multiple
```

## 可能遇到的问题及解决方案

1. **模块路径问题(JDK 9)**：
   ```bash
   # 如果遇到模块系统错误，尝试添加所有系统模块
   java --add-modules ALL-SYSTEM -cp target/classes cn.xiaxin.Multiple
   ```

2. **版本兼容性问题**：
   - 使用`-source`和`-target`选项确保向后兼容
   ```bash
   javac -source 8 -target 8 -d target/classes src/main/java/cn/xiaxin/Multiple.java
   ```

3. **类路径问题**：
   - 确保类路径正确包含所有依赖项

## 总结

直接使用`java.exe`命令运行Java类的基本步骤在JDK 8和JDK 9中是相同的，主要包括：
1. 使用`javac`编译源代码
2. 使用`java`运行编译后的类

主要区别在于：
- JDK 9引入了模块系统，可能需要额外的模块相关参数
- 某些API在JDK 9中可能已被标记为废弃
- 版本特定的代码可能需要条件编译或反射调用

通过创建简单的脚本，可以更方便地在不同JDK版本间切换和测试代码兼容性。
