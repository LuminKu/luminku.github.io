# 🎨在CentOS7中查看端口开放情况

在CentOS7中，查看端口开放情况可以通过防火墙命令来实现。以下是一些常用的命令和步骤：

### 1🎀查看已经开放的端口

要查看当前已经开放的端口，可以使用以下命令：

```bash
firewall-cmd --list-ports
```

该命令会列出所有已经开放的端口。

## 2🎨开启端口

如果需要开启一个新的端口，例如80端口，可以使用以下命令：
```bash
firewall-cmd --zone=public --add-port=80/tcp --permanent
```

命令解释：

--zone=public：指定作用域为public。

--add-port=80/tcp：添加80端口，使用TCP协议。

--permanent：永久生效，如果没有此参数，重启后设置会失效。

开启端口后，需要重启防火墙使设置生效：

firewall-cmd --reload


### 3👓查看端口占用情况

有时需要查看某个端口是否被占用，可以使用以下命令：

```bash
lsof -i tcp:80
```

该命令会列出占用80端口的进程。

### 4📋列出所有端口

要列出所有端口及其状态，可以使用以下命令：

```bash
netstat -ntlp
```

该命令会列出所有正在使用的端口及关联的进程/应用。

### 5🛠️关闭端口

如果需要关闭一个端口，例如80端口，可以使用以下命令：

```bash
firewall-cmd --zone=public --remove-port=80/tcp --permanent
```
然后重启防火墙使设置生效：
```bash
firewall-cmd --reload
```

通过以上命令和步骤，可以方便地在CentOS7中查看和管理端口的开放情况。