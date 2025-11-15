## CentOS 防火墙命令


在 CentOS 系统中，防火墙主要通过 **firewalld** 管理网络流量。以下是常用的防火墙命令及其操作方法。

### 1.基本操作

- **查看防火墙状态**：

```bash
systemctl status firewalld
```



- **启动防火墙**：

```bash
systemctl start firewalld
```



- **停止防火墙**：

```bash
systemctl stop firewalld
```



- **重启防火墙：**

```bash
systemctl restart firewalld.service
```

- **查看开发的端口：**

```bash
firewall-cmd --list-ports
```




- **设置开机自启**：

```bash
systemctl enable firewalld
```



- **禁用开机自启**：

```systemctl disable firewalldbash
systemctl disable firewalld
```

---

### 2.配置端口规则

- **查看已开放端口**：

```bash
firewall-cmd --zone=public --list-ports
```



- **开放指定端口（永久生效）**：

```bash
firewall-cmd --zone=public --add-port=80/tcp --permanent
```



- **移除指定端口（永久生效）**：

```bash
firewall-cmd --zone=public --remove-port=80/tcp --permanent
```



- **重新加载规则**：

```bash
firewall-cmd --reload
```

---

### 3. 配置服务规则

- **查看支持的服务列表**：

```bash
firewall-cmd --get-services
```



- **开放指定服务（如 HTTP）**：

```bash
firewall-cmd --zone=public --add-service=http --permanent
```



- **移除指定服务**：

```bash
firewall-cmd --zone=public --remove-service=http --permanent
```

### 4. 高级功能

- **启用 IP 地址伪装（用于 NAT 转发）**：

```bash
firewall-cmd --zone=external --add-masquerade
```



- **添加端口转发规则**： 将本地 *80* 转发到 *8080*：

```bash
firewall-cmd --zone=public --add-forward-port=port=80:proto=tcp:toport=8080 --permanent
```

---

### 5. 查看和管理区域

- **查看当前活动区域**：

```bash
firewall-cmd --get-active-zones
```




- **查看默认区域**：

```bash
firewall-cmd --get-default-zone
```

---

### 注意事项



1. 修改规则后，务必执行 *firewall-cmd --reload* 生效。
2. 在生产环境中更改防火墙配置时需谨慎，避免阻断必要的网络流量。

通过以上命令，您可以高效管理 CentOS 防火墙，确保系统的网络安全。