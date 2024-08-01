创建一个基于Ubuntu的Docker镜像以运行Caddy服务器是一个直接而有效的任务。以下Dockerfile提供了一个基本配置的例子，它会安装并运行Caddy服务器。请注意，Caddy的默认行为是作为一个静态文件服务器工作，若要扩展功能，可能需要额外配置Caddyfile。

```Dockerfile
# 使用Ubuntu作为基础镜像
FROM ubuntu:latest

# 添加维护者信息（可选）
LABEL maintainer="your_name"

# 环境变量，用于简化命令行操作，设置Caddy版本
ENV CADDY_VERSION=2.4.6

# 安装Caddy
# 更新软件包列表，安装curl和证书，然后清理缓存，减小镜像体积
RUN apt-get update \
    && apt-get install -y debian-keyring debian-archive-keyring apt-transport-https curl \
    && curl -1sLf 'https://dl.cloudsmith.io/public/caddy/stable/gpg.key' | gpg --dearmor -o /usr/share/keyrings/caddy-stable-archive-keyring.gpg \
    && curl -1sLf 'https://dl.cloudsmith.io/public/caddy/stable/debian.deb.txt' | tee /etc/apt/sources.list.d/caddy-stable.list \
    && apt-get update \
    && apt-get install -y caddy \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# 暴露Caddy默认端口
EXPOSE 80 443

# 运行Caddy
# 当不提供Caddyfile时，Caddy默认行为是作为静态文件服务器，
# 并在构建容器时设置的工作目录提供文件。
CMD ["caddy", "run", "--config", "/etc/caddy/Caddyfile", "--adapter", "caddyfile"]
```

这个Dockerfile执行以下操作：

1. **基础镜像**：以`ubuntu:latest`作为基础镜像。
2. **维护者信息**：`LABEL`指令用于添加维护者信息（此步骤是可选的）。
3. **环境变量**：设置了一个环境变量`CADDY_VERSION`来简化之后的命令行操作，如果Caddy版本更新，则需要更新这个变量。
4. **安装Caddy**：使用apt包管理器安装Caddy。
    - 首先，运行`apt-get update`更新软件包列表。
    - 然后，使用`curl`添加Caddy的GPG密钥和软件仓库。
    - 再次运行`apt-get update`以确保能够从新添加的Caddy仓库中检索软件包。
    - 使用`apt-get install`命令安装Caddy。
    - 最后，清理不再需要的文件，减少镜像大小。
5. **暴露端口**：暴露端口80和443，这些是Caddy监听HTTP和HTTPS请求的默认端口。
6. **运行Caddy**：设置默认命令来运行Caddy，这会使用`/etc/caddy/Caddyfile`作为配置文件。如果有特定的配置需求，需要将自定义的`Caddyfile`添加到镜像中，并相应地调整CMD指令。

请根据您具体的需求调整上述Dockerfile。创建Dockerfile之后，您可以通过以下命令来构建和运行您的Caddy容器：

```sh
docker build -t caddy-image .
docker run -d -p 80:80 -p 443:443 caddy-image
```

这将构建镜像并在后台运行Caddy服务器，映射端口80和443到主机，从而可以通过主机地址访问Caddy服务。
