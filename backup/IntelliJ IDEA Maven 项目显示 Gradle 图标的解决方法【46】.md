## IntelliJ IDEA Maven 项目显示 Gradle 图标的解决方法

在 IntelliJ IDEA 中，如果 Maven 项目意外显示 Gradle 图标，可能是由于项目配置文件或模块设置的问题导致的。以下是解决此问题的步骤。

示例问题

> Maven 项目中，pom.xml 文件图标显示为 Gradle 图标，而非正常的 Maven 图标。

解决方法

### 1. 检查项目类型

确保项目被正确识别为 Maven 项目，而不是 Gradle 项目。右键点击 *pom.xml* 文件，选择 **"Add as Maven Project"**，以重新加载项目为 Maven 类型。

### 2. 删除 Gradle 配置文件

如果项目中存在 *build.gradle* 或 *.gradle* 文件夹，可能会导致 IDEA 将其识别为 Gradle 项目。删除这些文件后，重新导入项目。

```bash
rm -rf build.gradle .gradle
```

### 3.清理并重新导入项目

在 IDEA 中执行以下操作：

- 打开 **File > Project Structure > Modules**。
- 删除错误的模块配置。
- 点击 **+** 按钮，重新添加正确的 Maven 模块。

### 4. 检查 IDEA 缓存

如果问题仍然存在，可以尝试清理 IntelliJ IDEA 的缓存：

- 打开 **File > Invalidate Caches / Restart**。
- 选择 **Invalidate and Restart**。

通过以上步骤，可以有效解决 Maven 项目显示 Gradle 图标的问题，使其恢复正常显示。