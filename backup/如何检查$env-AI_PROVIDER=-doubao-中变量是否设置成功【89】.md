在 Windows 操作系统中，你可以通过几种方式来检查环境变量 `AI_PROVIDER` 是否被正确设置为 `doubao`。这里介绍几种常用的方法：

方法1：使用命令提示符（CMD）

1. 打开命令提示符（CMD）。
2. 输入以下命令来查看环境变量：
   ```bash
   echo %AI_PROVIDER%
   ```
   如果环境变量设置成功，这将显示 `doubao`。如果没有设置，它将不显示任何内容或者显示一个空行。

方法2：使用 PowerShell

1. 打开 PowerShell。
2. 输入以下命令来查看环境变量：
   ```powershell
   echo $env:AI_PROVIDER
   ```
   或者
   ```powershell
   $env:AI_PROVIDER
   ```
   这也会显示 `doubao` 如果环境变量设置成功，否则将显示一个空行或者不显示任何内容。

方法3：通过设置环境变量查看

如果你不确定环境变量是否被正确设置，你可以在设置环境变量时同时检查它。这可以通过命令行来完成：

对于 CMD：
```bash
set AI_PROVIDER=doubao
echo %AI_PROVIDER%
```
这会将 `AI_PROVIDER` 设置为 `doubao` 并立即检查它。

对于 PowerShell：
```powershell
$env:AI_PROVIDER="doubao"
echo $env:AI_PROVIDER
```
或者更简单地：
```powershell
$env:AI_PROVIDER="doubao"
$env:AI_PROVIDER
```
这也会在设置后立即显示 `doubao`。

方法4：查看所有环境变量（可选）

如果你想查看系统中所有的环境变量，可以使用以下命令：

对于 CMD：
```bash
set
```
这将列出所有的环境变量及其值。你可以在其中搜索 `AI_PROVIDER` 来确认它的值。

对于 PowerShell：
```powershell
Get-ChildItem Env:
```
或者更具体地：
```powershell
$env: | Out-String -Stream | Select-String "AI_PROVIDER"
```
这些方法可以帮助你确认 `AI_PROVIDER` 环境变量是否已经正确设置为 `doubao`。如果你在设置后仍然看不到期望的值，可能需要检查你是否在正确的用户或系统环境变量中设置了该值。在 Windows 中，用户级别的环境变量可以在当前用户的会话中生效，而系统级别的环境变量对所有用户都有效。确保你在正确的级别进行了设置。