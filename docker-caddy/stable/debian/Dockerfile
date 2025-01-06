# 基础镜像选用Debian最新稳定版
FROM debian:stable-slim

# 维护者信息
LABEL maintainer="jumploop"

# 环境变量，用于简化命令行操作，比如可用于指定Caddy的版本
ENV CADDY_VERSION=2.4.6

# 安装Caddy
# 由于我们使用的是Debian，直接通过官方指南下载并安装Caddy
RUN apt-get update  && \
    apt-get install -y debian-keyring debian-archive-keyring apt-transport-https curl && \
    curl -1sLf 'https://dl.cloudsmith.io/public/caddy/stable/gpg.key' | gpg --dearmor -o /usr/share/keyrings/caddy-stable-archive-keyring.gpg && \
    curl -1sLf 'https://dl.cloudsmith.io/public/caddy/stable/debian.deb.txt' | tee /etc/apt/sources.list.d/caddy-stable.list && \
    apt-get update && \
    apt-get install -y caddy && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# 暴露Caddy默认端口
EXPOSE 80 443

# 设置默认命令：运行Caddy
CMD ["caddy", "run", "--config", "/etc/caddy/Caddyfile", "--adapter", "caddyfile"]
