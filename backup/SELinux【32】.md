> **SELinux**（Security-Enhanced Linux）是一个Linux内核模块，也是一个安全子系统。它由美国国家安全局（NSA）开发，旨在通过**强制访问控制**（MAC）来增强系统的安全性。

### 1🎋SELinux的作用

SELinux的主要作用是**最小化系统中服务进程可访问的资源**，即最小权限原则。它通过限制进程对资源的访问权限，即使进程以root身份运行，也只能访问其所需的资源，从而提高系统的安全性。

### 2🎨SELinux的工作原理

SELinux通过**标签**（即安全上下文）来进行访问控制。每个进程和文件都有各自的标签，SELinux根据这些标签和预定义的规则来决定是否允许访问。

### 3🚨安全上下文

安全上下文由四个字段组成：SELinux用户、角色、类型和敏感度级别。例如，*system_u:object_r:admin_home_t:s0*。

**类型强制规则**

SELinux的核心是**类型强制**（Type Enforcement, TE）模型。TE模型基于主体（进程）、操作（如读、写）和客体（如文件、端口）来定义规则。例如，*allow user_t bin_t:file { read write };*表示允许类型为*user_t*的进程读取或写入类型为*bin_t*的文件。

SELinux**的工作模式**

SELinux有三种工作模式：

1. **enforcing**：强制模式，违反规则的行为将被阻止并记录到日志中。
2. **permissive**：宽容模式，违反规则的行为只会记录到日志中。
3. **disabled**：关闭SELinux。

### 4🛠️SELinux的基本操作

==查询文件或目录的安全上下文==

```bash
ls -Z /etc/hosts
```

==查询进程的安全上下文==

```bash
ps auxZ | grep nginx
```

==修改文件或目录的安全上下文==

```bash
chcon -u user_u -r role_r -t type_t file
```

==恢复文件或目录的默认安全上下文==

```bash
restorecon -R /path/to/directory
```

查询和设置布尔型规则

==查询所有规则：==

```bash
getsebool -a
```

==设置规则：==

```bash
setsebool -P httpd_anon_write on
```

SELinux通过这些机制和操作，提供了一个强大的安全框架，帮助管理员更好地保护系统。