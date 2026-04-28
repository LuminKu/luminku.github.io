## DEA中设置日志打印颜色

<img width="2494" height="347" alt="Image" src="https://github.com/user-attachments/assets/76229652-03a0-4bbd-b1af-815fda5c6106" />

在 **IntelliJ IDEA** 中为控制台日志设置颜色，可以提高日志的可读性和开发效率。以下是实现方法的详细步骤。

### 方法一：使用 GrepConsole 插件

1. **安装插件** 打开 IDEA，依次点击 File > Settings > Plugins。 搜索 GrepConsole 插件并安装。 安装完成后，重启 IDEA。
2. **配置日志颜色** 进入 File > Other Settings > Grep Console。 在设置界面中，为不同级别的日志（如 INFO、WARN、ERROR）选择对应的颜色。
3. **验证效果** 启动项目，查看控制台输出，日志会按照配置显示不同的颜色。

### 方法二：通过 VM Options 设置

- **编辑配置** 点击右上角的 Edit Configurations 按钮。 选择需要配置的项目或模块，点击 Modify options，选择 Add VM options。
- **添加参数** 在 VM Options 输入框中添加以下参数： -Dspring.output.ansi.enabled=ALWAYS 点击 OK 并重启项目。
- **查看效果** 控制台会根据日志级别（INFO、WARN、ERROR）显示不同颜色。

### 方法三：手动设置控制台颜色

1. **打开设置界面** 依次点击 File > Settings > Editor > Color Scheme > General。
2. **配置颜色** 找到 Console Output 下的选项，如 Error messages、Warning messages 等。 为每个选项选择合适的颜色并保存设置。
3. **应用更改** 点击 Apply 和 OK，然后运行项目查看彩色日志效果。

提示与注意事项

- 如果需要更美观的主题，可以安装 **Material Theme UI** 插件。
- 不同方法适用于不同需求，可根据实际情况选择合适方案。