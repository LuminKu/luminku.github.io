解决 IDEA 控制台中文乱码的核心是统一编码格式为 UTF-8，并通过修改虚拟机配置强制生效。
基础编码设置

1. 修改全局文件编码：进入 File -> Settings -> Editor -> File Encodings，将 Global Encoding、Project Encoding 及 Default encoding for properties files 全部设置为UTF-8。
2. 配置 VM 选项：点击 Help -> Edit Custom VM Options，在文件末尾添加 `-Dfile.encoding=UTF-8`，保存后重启 IDEA。
   - 注意：建议通过菜单编辑，直接修改安装目录下的配置文件可能无效 。
特定场景配置

1. Tomcat 项目：若使用 Tomcat，进入 Run -> Edit Configurations，在对应的 VM options 中添加 `-Dfile.encoding=UTF-8`。
2. 避免错误修改：不建议修改 Tomcat 的 logging.properties 为 GBK，应保持 UTF-8 并强制 IDEA 显示，否则可能导致参数乱码 。
其他检查

1. 字体设置：进入 Settings -> Appearance，将字体设置为微软雅黑等支持中文的字体，避免字体缺失导致乱码 。
2. JDK 兼容性：若上述方法无效，检查 JDK 版本，尝试更换为 JDK 17 等稳定版本，排除 JDK 内置编码冲突 。

【附加参考】
解决步骤：
1. 打开cmd输入命令 chcp 65001 设置临时窗口的编码为UTF-8。控制台日志不会乱码了
2. 启动时指定编码:java -Dfile.encoding=utf-8 -jar xxx.jar 返回数据不会乱码了