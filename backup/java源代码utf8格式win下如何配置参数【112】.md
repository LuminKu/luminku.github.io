在Windows环境下，使用Java源代码时，确保你的源代码文件（.java文件）和最终的编译后的字节码文件（.class文件）都以UTF-8编码保存，是非常重要的，特别是当你需要处理国际化或包含非ASCII字符（如中文、日文等）时。以下是一些关键的步骤和配置建议：

1. 确保你的文本编辑器支持UTF-8

首先，确保你使用的文本编辑器（如Notepad++, Visual Studio Code, Sublime Text等）支持并设置为UTF-8编码。

Notepad++
- 打开Notepad++。
- 点击顶部菜单的“格式(L)” -> “UTF-8无BOM编码(U)”。

Visual Studio Code
- 打开VS Code。
- 打开文件后，点击右下角的编码格式（如“C”图标），选择“以UTF-8编码重新打开”。

2. 配置Java编译器使用UTF-8

当你使用命令行编译Java代码时，确保编译器使用UTF-8编码。可以通过以下方式配置：

使用`javac`命令
在命令行中，你可以通过添加`-encoding`参数来指定编码方式：

```bash
javac -encoding UTF-8 MyClass.java
```

3. 配置IDE（如IntelliJ IDEA, Eclipse等）使用UTF-8

大多数现代IDE都允许你为项目或全局设置文件编码。

IntelliJ IDEA
- 打开IDEA，进入“File” -> “Settings”（或“IntelliJ IDEA” -> “Preferences”在Mac上）。
- 在“Settings”窗口中，选择“Editor” -> “File Encodings”。
- 在“Global Encoding”和“Project Encoding”下选择“UTF-8”。
- 确保“Default encoding for properties files”也是“UTF-8”。

Eclipse
- 打开Eclipse，点击“Window” -> “Preferences”。
- 在“Preferences”窗口中，选择“General” -> “Workspace”。
- 在“Text file encoding”下拉菜单中选择“Other: UTF-8”，然后点击“Apply and Close”。

4. 确保运行环境支持UTF-8

当你的Java程序运行时，确保JVM（Java虚拟机）也使用UTF-8编码。可以通过以下方式设置：

```bash
java -Dfile.encoding=UTF-8 -jar yourapp.jar
```
或者在你的IDE中设置运行配置：

- 在IntelliJ IDEA中，右键点击运行/调试配置，选择“Edit Configurations...”，在“VM options”中添加`-Dfile.encoding=UTF-8`。
- 在Eclipse中，右键点击运行配置，选择“Run Configurations...”，在“Arguments”标签页的“VM arguments”中添加`-Dfile.encoding=UTF-8`。

按照这些步骤操作后，你的Java项目应该能够在Windows环境下正确处理UTF-8编码的源代码和资源文件。