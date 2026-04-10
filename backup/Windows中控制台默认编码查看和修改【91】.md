在 Windows 系统中，查看和修改控制台编码主要通过命令行工具（如 CMD 或 PowerShell）进行。以下是具体操作方法：

---

查看当前控制台编码

- CMD（命令提示符）  
  打开 CMD，输入以下命令：  
  ```cmd
  chcp
  ```  
  输出示例：`Active code page: 936`  
  - 936 表示 GBK（简体中文默认编码）  
  - 65001 表示 UTF-8 编码  

- PowerShell  
  打开 PowerShell，输入：  
  ```powershell
  [Console]::OutputEncoding
  ```  
  若输出包含 `UTF-8`，则当前为 UTF-8 编码。

---

临时修改编码（仅当前会话有效）

- CMD 中切换为 UTF-8  
  ```cmd
  chcp 65001
  ```

- PowerShell 中设置为 UTF-8  
  ```powershell
  [Console]::OutputEncoding = [System.Text.Encoding]::UTF8
  ```

---

永久修改默认编码

> ⚠️ 永久修改需谨慎，建议优先使用临时方案或在程序中显式设置编码。

- 方法一：通过注册表设置 CMD 默认编码（推荐）  
  1. 按 `Win + R`，输入 `regedit` 回车。  
  2. 导航至：  
     ```
     HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Command Processor
     ```  
  3. 右键 → 新建 → 字符串值，命名为 `autorun`。  
  4. 双击 `autorun`，数值数据填入：  
     ```
     chcp 65001 > nul
     ```  
     （`> nul` 可隐藏提示信息）  
  5. 重启 CMD 生效。

- 方法二：启用系统级 UTF-8 支持（Windows 10/11）  
  1. 打开 设置 → 时间和语言 → 语言和区域。  
  2. 点击 管理语言设置 下的 更改系统区域设置。  
  3. 勾选 “使用 Unicode UTF-8 提供全球语言支持”。  
  4. 重启电脑 生效。  
     此方法会影响整个系统，适用于多语言环境 。

---

IDE 终端编码设置（如 VS Code、IntelliJ IDEA）

- VS Code  
  在 `settings.json` 中添加：  
  ```json
  "terminal.integrated.shellArgs.windows": ["-NoExit", "/c", "chcp 65001"],
  "files.encoding": "utf8"
  ```

- IntelliJ IDEA  
  进入 File → Settings → Editor → File Encodings，将 IDE Encoding 和 Project Encoding 均设为 UTF-8 。

---

注意事项
- 临时修改在关闭终端后失效。
- 永久修改可能影响其他依赖系统默认编码的程序。
- 若使用 Git Bash、WSL 等非原生终端，其编码规则可能不同，需单独配置 。

如需进一步排查乱码问题，可结合文件编码与终端编码一致性检查 。