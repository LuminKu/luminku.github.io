# Lua 命令行执行 Lua 文件


在命令行中运行 Lua 文件是一种快速测试和执行脚本的方式。以下是具体步骤：

1. 确保已安装 Lua

- 在运行 Lua 文件之前，请确保已安装 Lua 环境。

- 在命令行中输入以下命令检查版本：

```bash
lua -v
```

2. 创建 Lua 文件

- 使用任意文本编辑器创建一个 *.lua* 文件，例如 *hello.lua*。

- 在文件中编写简单代码，例如：

```bash
print("Hello, World!")
```


3. 使用命令行运行 Lua 文件

- 打开命令行工具（如 CMD、终端）。

- 导航到 Lua 文件所在的目录，例如：

```bash
cd /path/to/your/lua/file
```


- 输入以下命令运行 Lua 文件：

```bash
lua hello.lua
```


4. 调试模式运行

- 如果需要在执行脚本后进入交互模式，可以使用 *-i* 参数：

```bash
lua -i hello.lua
```


5. 在 Linux/Mac 中直接运行

- 在文件顶部添加以下 shebang 声明：

```bash
#!/usr/bin/env lua

print("Hello, World!")
```

- 保存后赋予文件可执行权限：

```bash
chmod +x hello.lua
```

- 然后直接运行文件：

```bash
./hello.lua
```


提示与注意事项

- 使用 *dofile("filename.lua")* 可在交互模式中加载并运行文件。
- 确保路径正确，避免因路径问题导致无法找到文件。

通过以上方法，您可以轻松在命令行中运行 Lua 文件并调试代码。