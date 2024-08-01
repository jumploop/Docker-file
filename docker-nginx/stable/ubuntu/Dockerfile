# 使用最新的Ubuntu镜像作为基础
FROM ubuntu:latest

# 定义维护者信息（可选）
LABEL maintainer="jumploop"

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
