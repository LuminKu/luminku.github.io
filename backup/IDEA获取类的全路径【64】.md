在使用 **IntelliJ IDEA** 进行开发时，获取类的全路径（包括包名）是一个常见需求，特别是在大型项目中。这可以帮助开发者快速定位类的位置并提高工作效率。以下是几种实现方法：

**方法一：通过类顶部的包名查看**

- 打开目标类文件。

- 在文件顶部，查看 *package* 声明。例如： *package com.example.myproject.models; public class User { // ... }* 此处 com.example.myproject.models.User 即为该类的全路径。

**方法二：使用快捷键快速查找**

1. 按下 **Ctrl + N**（Windows/Linux）或 **Command + O**（macOS）。
2. 输入类名，IDEA会自动补全并显示匹配结果。
3. 选中目标类并按回车，顶部会显示完整路径。

**方法三：复制全路径**

1. 打开目标类文件。
2. 选中方法或类名。
3. 按下快捷键 **Ctrl + Shift + Alt + C**（Windows/Linux）或对应的 macOS 快捷键。
4. IDEA会将完整路径（如 *com.example.myproject.models.User#methodName*）复制到剪贴板。

**方法四：查看文件绝对路径**

1. 右键点击目标文件，在菜单中选择 **Copy Path/Reference**。
2. 选择 **Absolute Path** 或 **From Content Root**，即可复制文件的绝对路径或相对路径。

小提示

- 如果需要频繁查看路径，可以通过设置快捷键或插件优化操作流程。
- 确保项目结构清晰，便于快速定位和管理类文件。

通过以上方法，无论是查看、复制还是定位类的全路径，都能轻松实现！