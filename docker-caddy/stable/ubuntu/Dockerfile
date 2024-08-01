# 使用Ubuntu作为基础镜像
FROM ubuntu:latest

# 添加维护者信息（可选）
LABEL maintainer="jumploop"

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
