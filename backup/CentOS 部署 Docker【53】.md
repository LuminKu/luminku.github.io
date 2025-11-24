以下是在 CentOS 系统上安装和配置 Docker 的详细步骤，确保您可以快速部署并正常运行。

## 1. 卸载旧版本

如果系统中已存在旧版本的 Docker，请先卸载：

```bash
yum remove docker \
docker-client \
docker-client-latest \
docker-common \
docker-latest \
docker-latest-logrotate \
docker-logrotate \
docker-engine
```



## 2. 安装必要工具

安装 yum-utils 以便管理软件包：

```bash
yum install -y yum-utils
```



## 3. 配置 Docker 仓库

添加 Docker 官方或阿里云镜像源：

```bash
yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
# 或使用阿里云镜像源（推荐国内用户）
yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
```



## 4. 安装 Docker

执行以下命令安装 Docker：

```bash
yum install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```



## 5. 启动并验证 Docker

启动 Docker 服务并设置开机自启：

```bash
systemctl start docker
systemctl enable docker
```

验证安装是否成功：

```bash
docker run hello-world
```



如果看到 "Hello from Docker!" 的输出，说明安装成功。

## 6. 配置镜像加速器（可选）

为提高国内拉取镜像速度，可配置阿里云加速器：

```bash
mkdir -p /etc/docker

tee /etc/docker/daemon.json <<-'EOF'
{
"registry-mirrors": ["https://<你的加速器地址>.mirror.aliyuncs.com"]
}
EOF

systemctl daemon-reload
systemctl restart docker
```

## 7. 常用命令

查看运行中的容器：docker ps

列出本地镜像：docker images

停止容器：docker stop <容器ID>

删除容器：docker rm <容器ID>

通过以上步骤，您已成功在 CentOS 上部署了 Docker，并完成了基础配置。