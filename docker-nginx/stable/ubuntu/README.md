要创建一个基于Ubuntu的Docker镜像以运行Nginx，你可以遵循以下步骤来编写Dockerfile。下面是一个简单的例子，显示了如何制作包含Nginx的Docker镜像。

```dockerfile
# 使用最新的Ubuntu镜像作为基础
FROM ubuntu:latest

# 定义维护者信息（可选）
LABEL maintainer="yourname@example.com"

# 安装Nginx
# - 更新系统包索引
# - 安装Nginx包
# - 清除缓存，减小镜像大小
RUN apt-get update && \
    apt-get install -y nginx && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# 定义运行时使用的端口
EXPOSE 80 443

# 使用非守护进程方式运行Nginx
# 这是在Docker容器中运行服务的推荐方式
CMD ["nginx", "-g", "daemon off;"]
```

这个Dockerfile执行以下主要步骤：

1. **基础镜像**：选用`ubuntu:latest`作为基础镜像。
2. **维护者信息**：通过`LABEL`指令设置维护者信息，这是一个可选步骤。
3. **安装Nginx**：使用`apt-get`命令更新Ubuntu的包索引，安装Nginx，并清理安装过程中的临时文件来减少镜像大小。
4. **端口**：通过`EXPOSE`指令声明容器在运行时应该监听的端口80和443。这对于容器的使用者来说是个有用的文档化方式，并不会实际上发布端口或影响容器运行时的端口映射。
5. **启动命令**：使用`CMD`指令设置容器启动时执行的默认命令，用来启动Nginx服务。`nginx -g "daemon off;"`命令确保Nginx在前台运行，这对于Docker容器来说是必需的，因为Docker被设计为运行单个前台进程。

完成编写Dockerfile后，你可以通过以下命令来构建并运行你的Nginx容器：

```bash
# 构建Docker镜像
docker build -t nginx-image .

# 运行Docker容器
docker run -d -p 80:80 -p 443:443 nginx-image
```

这将在后台启动一个Nginx容器并将容器的80和443端口映射到主机的相应端口上，使得你可以通过浏览器访问Nginx服务器。
