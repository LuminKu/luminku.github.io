## Linux 查进程ID

在 Linux 系统中，查找和查看进程的 ID（PID）是管理和监控系统的重要任务。以下是几种常用方法：

### 1. 使用 ps 命令

*ps* 是最常用的命令之一，用于列出当前运行的进程及其详细信息。

**示例：**

```bash
ps aux | grep <进程名>
```



- *ps aux* 列出所有进程。
- *grep <进程名>* 用于筛选目标进程。

**输出示例：**

```bash
root 1234 0.0 0.1 123456 7890 ? S 09:30 0:01 /usr/bin/python app.py
```



- 第一列为用户，第二列为 **PID**（进程ID）。

------

### 2. 使用 pidof 命令

*pidof* 可直接获取指定程序的 PID。

**示例：**

```bash
pidof <程序名>
```



**输出示例：**

```plain text
1234
```

------

### 3. 查看 /proc 文件系统

通过 */proc* 文件系统可以直接访问进程信息。

**示例：**

```bash
cat /proc/<PID>/status
```



- 查看进程状态、内存使用等详细信息。

**其他命令：**

- 查看启动命令：

```bash
cat /proc/<PID>/cmdline
```



- 查看可执行文件路径：

```bash
ls -l /proc/<PID>/exe
```

------

### 4. 使用 top 或 htop 实时监控

- **top 命令：**

```bash
top -p <PID>
```



- **htop 命令：** 更直观的交互式界面（需安装）。

------

### 注意事项

- 如果需要查看其他用户的进程，可能需要使用 *sudo* 权限：

```bash
sudo ps -p <PID>
```



- 若无法找到 PID，请确认进程是否存在：

```bash
ps aux | grep <关键词>
```



通过以上方法，可以快速查找和分析 Linux 系统中的进程 ID。