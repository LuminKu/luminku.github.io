> **CMD（命令提示符）** 是 Windows 自 DOS 时代延续下来的传统命令行工具，主要处理**纯文本**，适合执行简单的文件操作、批处理脚本等基础任务，启动快、占用资源少，但功能相对有限，缺乏现代化的脚本和对象处理能力。

**PowerShell** 则是微软推出的**面向对象**命令行与脚本语言，基于 **.NET Framework / .NET Core** 构建，输出的是**结构化对象**而非纯文本，可直接访问对象属性和方法，支持强大的**管道传递对象**、模块化、跨平台（Windows、Linux、macOS）以及高级自动化运维功能。

**核心差异**

- **数据处理**：CMD 处理文本流，PowerShell 处理对象流。
- **脚本能力**：CMD 支持 *.bat* 批处理，PowerShell 支持 *.ps1* 脚本，具备条件判断、循环、异常处理等高级特性。
- **管道机制**：CMD 管道传递文本，PowerShell 管道传递对象，避免解析文本的二次处理。
- **跨平台**：CMD 仅限 Windows，PowerShell 7+ 可跨平台。

**示例对比：获取内存占用最高的前 5 个进程**

```CMD

tasklist /fo csv | findstr /v "Image Name" | sort /r /+5 | more +1
```

```PowerShell
Get-Process | Sort-Object WorkingSet -Descending | Select-Object -First 5
```

PowerShell 语法更简洁，且每个进程是完整对象，可直接访问属性。

**适用场景建议**

- **CMD**：快速执行简单命令、兼容旧脚本、资源受限环境。
- **PowerShell**：系统管理、批量运维、数据处理、跨平台自动化。

**进阶技巧（PowerShell）**

- **管道操作**：*Get-Service | Where-Object {$_.Status -eq "Running"}*
- **模块化**：*Install-Module Az* 管理 Azure 资源
- **远程管理**：*Invoke-Command -ComputerName Server01 -ScriptBlock { Get-Process }*

结合实际需求选择工具：**简单任务用 CMD，高级自动化用 PowerShell**，两者配合能覆盖从轻量操作到企业级运维的全场景。