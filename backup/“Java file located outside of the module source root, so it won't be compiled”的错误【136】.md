在Java开发中，如果你遇到了“Java file located outside of the module source root, so it won't be compiled”的错误，这通常意味着你的Java文件没有被包含在你的模块的源根目录中。源根目录是编译器查找源代码文件的地方。解决这个问题的方法通常有以下几种：

1. 添加源根目录
确保你的项目配置正确，包括源根目录的配置。这可以通过IDE（如IntelliJ IDEA、Eclipse等）或者通过构建工具（如Maven、Gradle）来实现。

使用IDE：
- IntelliJ IDEA:
  1. 打开项目设置（`File` > `Project Structure`）。
  2. 在`Modules`选项卡中，选择你的模块。
  3. 在`Sources`部分，点击`+`号添加一个新的源根目录。
  4. 浏览到你的Java文件所在的目录，选择它，然后点击`OK`。

- Eclipse:
  1. 右键点击项目，选择`Properties`。
  2. 在`Java Build Path`页面，选择`Source`标签。
  3. 点击`Add Folder`，然后选择你的Java文件所在的目录。
  4. 点击`Apply and Close`。

使用Maven或Gradle：
- Maven:
  在`pom.xml`文件中，确保你的源目录配置正确：
  ```xml
  <build>
      <sourceDirectory>src/main/java</sourceDirectory>
  </build>
  ```

- Gradle:
  在`build.gradle`文件中，配置源目录：
  ```groovy
  sourceSets {
      main {
          java {
              srcDirs = ['src/main/java']
          }
      }
  }
  ```

2. 移动文件到源根目录
如果你不想改变项目的源根目录设置，你可以选择将Java文件移动到当前配置的源根目录中。右键点击文件或文件夹，选择`Move`或`Refactor`，然后选择一个新的位置在你的源根目录内。

3. 检查`.iml`文件（IntelliJ IDEA）
如果你在使用IntelliJ IDEA，有时候`.iml`文件（模块文件）可能会损坏或者不正确配置。你可以尝试重新加载项目或者手动编辑`.iml`文件来修复这个问题。例如，确保`<content url="file://$MODULE_DIR$">`标签下包含了正确的源目录：
```xml
<content url="file://$MODULE_DIR$">
    <sourceFolder url="file://$MODULE_DIR$/src/main/java" isTestSource="false" />
</content>
```

通过以上步骤，你应该能够解决“Java file located outside of the module source root, so it won't be compiled”的错误。如果问题仍然存在，检查是否有其他的配置错误或者IDE的缓存问题，可能需要重启IDE或者清理项目。