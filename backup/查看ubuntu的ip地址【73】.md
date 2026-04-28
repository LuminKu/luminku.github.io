在Ubuntu系统中，有多种方法可以查看IP地址。以下是一些常用的方法：

1. 使用`ip`命令

打开终端，然后输入以下命令之一：

- 查看所有网络接口的IP地址：
  ```
  ip addr
  ```
  或者
  ```
  ip a
  ```

- 查看所有IPv4地址：
  ```
  ip -4 addr
  ```

- 查看所有IPv6地址：
  ```
  ip -6 addr
  ```

2. 使用`ifconfig`命令

虽然`ifconfig`命令在新版的Ubuntu中可能默认不安装，你可以通过安装`net-tools`包来使用它：

```bash
sudo apt-get install net-tools
```

安装完成后，使用以下命令查看IP地址：
```bash
ifconfig
```
这将列出所有网络接口及其IP地址。你也可以通过指定接口名来查看特定接口的IP地址，例如：
```bash
ifconfig eth0
```
这里`eth0`是网络接口的名称，根据你的系统配置可能有所不同。

3. 使用`hostname`命令结合`-I`选项

这个命令可以直接显示主机的IP地址：
```bash
hostname -I
```
这将会返回一个或多个IP地址，取决于你的网络配置。

4. 使用`nmcli`命令（NetworkManager CLI）

如果你的系统使用NetworkManager管理网络，可以使用`nmcli`命令查看IP地址：
```bash
nmcli device show <device> | grep IP4.ADDRESS
```
或者查看所有设备的IP地址：
```bash
nmcli con show | grep 'ipv4.address'
```
在这里，`<device>`是你想要查询的网络设备名称，例如`eth0`或`wlp3s0`。如果不指定设备，可以使用`nmcli device status`查看所有设备的状态和名称。

5. 使用图形界面（如果可用）

在Ubuntu的图形界面中，你也可以通过“设置”->“网络”来查看IP地址。点击你的网络连接（例如以太网或WiFi），然后选择“IPv4”或“IPv6”标签页，你将看到分配给你的IP地址。

选择适合你当前环境和偏好的方法，你应该能够轻松地查找到Ubuntu系统的IP地址。