在CentOS中，你可以通过以下步骤来开启、查看和关闭指定的端口：

### 开启端口

1. **使用`firewall-cmd`命令（如果你的系统使用的是firewalld）**：
   - 开启端口7001、7002、7003：
     ```bash
     firewall-cmd --zone=public --add-port=7001/tcp --permanent
     firewall-cmd --zone=public --add-port=7002/tcp --permanent
     firewall-cmd --zone=public --add-port=7003/tcp --permanent
     ```
   - 重新加载firewalld以使更改生效：
     ```bash
     firewall-cmd --reload
     ```

2. **使用`iptables`命令（如果你的系统使用的是iptables）**：
   - 开启端口7001、7002、7003：
     ```bash
     iptables -A INPUT -p tcp --dport 7001 -j ACCEPT
     iptables -A INPUT -p tcp --dport 7002 -j ACCEPT
     iptables -A INPUT -p tcp --dport 7003 -j ACCEPT
     ```
   - 保存iptables规则：
     ```bash
     service iptables save
     ```
   - 重新加载iptables规则：
     ```bash
     systemctl restart iptables
     ```

### 查看是否生效

1. **使用`firewall-cmd`命令**：
   - 查看开放的端口：
     ```bash
     firewall-cmd --list-all
     ```

2. **使用`iptables`命令**：
   - 查看iptables规则：
     ```bash
     iptables -L
     ```

### 关闭端口

1. **使用`firewall-cmd`命令**：
   - 关闭端口7001、7002、7003：
     ```bash
     firewall-cmd --zone=public --remove-port=7001/tcp --permanent
     firewall-cmd --zone=public --remove-port=7002/tcp --permanent
     firewall-cmd --zone=public --remove-port=7003/tcp --permanent
     ```
   - 重新加载firewalld以使更改生效：
     ```bash
     firewall-cmd --reload
     ```

2. **使用`iptables`命令**：
   - 删除之前添加的规则：
     ```bash
     iptables -D INPUT -p tcp --dport 7001 -j ACCEPT
     iptables -D INPUT -p tcp --dport 7002 -j ACCEPT
     iptables -D INPUT -p tcp --dport 7003 -j ACCEPT
     ```
   - 保存iptables规则：
     ```bash
     service iptables save
     ```
   - 重新加载iptables规则：
     ```bash
     systemctl restart iptables
     ```

请根据你的系统配置选择合适的命令。如果你不确定你的系统使用的是`firewalld`还是`iptables`，可以通过以下命令来检查：

- 检查`firewalld`是否正在运行：
  ```bash
  systemctl status firewalld
  ```
  
- 检查`iptables`是否正在运行：
  ```bash
  systemctl status iptables
  ```