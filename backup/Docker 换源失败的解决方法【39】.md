## Docker 换源失败的解决方法
当 Docker 无法换源时，通常是由于配置错误或镜像加速器地址不可用导致的。以下是解决此问题的步骤和示例。

示例问题

```bash
docker pull golang:1.22-alpine

# 错误信息：

# Get "https://registry-1.docker.io/v2/": net/http: request canceled while waiting for connection

```

1. 检查并配置加速器

编辑 */etc/docker/daemon.json* 文件（如果不存在可以创建），添加国内可用的镜像加速器地址。

**示例配置：**

```json
{

"registry-mirrors": [

    "https://docker.m.daocloud.io",

    "https://hub-mirror.c.163.com",

    "https://registry.cn-hangzhou.aliyuncs.com"

  ]

}
```




保存后，执行以下命令重启 Docker 服务：

```bash
sudo systemctl daemon-reload

sudo systemctl restart docker
```



通过 *docker info* 检查是否成功应用加速器：

```bash
docker info | grep "Registry Mirrors"
```


2. 清理缓存并重试

如果换源后仍然无法拉取镜像，可以尝试清理 Docker 缓存：

```bash
docker system prune -a
```

然后重新拉取镜像：

```bash
docker pull golang:1.22-alpine
```


3. 替换镜像仓库前缀

某些情况下，直接替换镜像仓库前缀可以解决问题。例如，将 *docker.io* 替换为国内服务商的地址：

```bash
docker pull docker.m.daocloud.io/library/golang:1.22-alpine
```


4. 检查网络连接和代理设置

确保服务器能够访问镜像加速器地址。如果使用了代理，请检查代理配置是否正确。

通过以上方法，可以有效解决 Docker 换源失败的问题，提高镜像拉取速度。