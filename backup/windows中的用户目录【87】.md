## [[获取 Windows 当前用户目录](https://www.cnblogs.com/egan123/p/10115639.html)](https://www.cnblogs.com/egan123/p/10115639.html)

在 Windows 系统中，获取当前用户目录是一个常见的需求，尤其是在需要操作用户文件时。通过环境变量 `%USERPROFILE%` 可以轻松获取当前用户的目录。

#### 使用 CMD 获取当前用户目录

```bash
echo %USERPROFILE%
```

此命令将输出当前用户的目录路径，例如 `C:\Users\YourUsername`。

常用的系统路径环境变量

除了 `%USERPROFILE%`，还有一些常用的系统路径环境变量，可以帮助你获取其他重要的系统目录：

- `%WINDIR%`：系统目录，例如 `C:\WINDOWS`
- `%SYSTEMROOT%`：系统根目录，例如 `C:\WINDOWS`
- `%SYSTEMDRIVE%`：系统盘符，例如 `C:`
- `%HOMEDRIVE%`：当前用户根目录，例如 `C:`
- `%HOMEPATH%`：当前用户路径，例如 `\Users\YourUsername`
- `%TMP%` 和 `%TEMP%`：当前用户临时文件夹，例如 `C:\Users\YourUsername\AppData\Local\Temp`
- `%APPDATA%`：当前用户应用数据文件夹，例如 `C:\Users\YourUsername\AppData\Roaming`
- `%PROGRAMFILES%`：程序默认安装目录，例如 `C:\Program Files`
- `%COMMONPROGRAMFILES%`：文件通用目录，例如 `C:\Program Files\Common Files`
- `%USERNAME%`：当前用户名，例如 `YourUsername`
- `%ALLUSERSPROFILE%`：所有用户文件目录，例如 `C:\ProgramData`
- `%COMPUTERNAME%`：计算机名，例如 `YourComputerName`
- `%NUMBER_OF_PROCESSORS%`：处理器个数，例如 `4`
- `%PROCESSOR_ARCHITECTURE%`：处理器架构，例如 `AMD64`
- `%PROCESSOR_LEVEL%`：处理器型号，例如 `6`
- `%PROCESSOR_REVISION%`：处理器修订号，例如 `0905`
- `%USERDOMAIN%`：包含用户帐号的域，例如 `YourDomain`
- `%PATH%`：系统搜索路径

这些环境变量可以通过 `echo` 命令在 CMD 中查看，例如：


```
echo %WINDIR%
```


总结

通过使用环境变量 `%USERPROFILE%`，可以方便地获取当前用户的目录路径。此外，了解其他常用的系统路径环境变量也有助于更好地管理和操作系统文件。希望这些信息对你有所帮助。