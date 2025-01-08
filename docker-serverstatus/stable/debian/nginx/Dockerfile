# The Dockerfile for build localhost source, not git repo
FROM debian:stable-slim AS builder

LABEL maintainer="jumploop"
ARG WORK=/root
ARG BRANCH=master

RUN apt-get update  \
    && apt-get -y install gcc g++ make libcurl4-openssl-dev wget unzip  \
    && wget -q --no-check-certificate https://github.com/jumploop/ServerStatus/archive/refs/heads/$BRANCH.zip -P $WORK  \
    && unzip $WORK/$BRANCH.zip -d $WORK  \
    && rm -rf $WORK/*.zip \
    && cd $WORK/ServerStatus-$BRANCH/server \
    && make -j \
    && pwd && ls -a \
    && mv $WORK/ServerStatus-$BRANCH/* /


# glibc env run
FROM nginx:latest

RUN mkdir -p /ServerStatus/server/ && ln -sf /dev/null /var/log/nginx/access.log && ln -sf /dev/null /var/log/nginx/error.log

COPY --from=builder server /ServerStatus/server/
COPY --from=builder web /usr/share/nginx/html/

# china time
ENV TZ=Asia/Shanghai
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

EXPOSE 80 35601
HEALTHCHECK --interval=5s --timeout=3s --retries=3 CMD curl --fail http://localhost:80 || bash -c 'kill -s 15 -1 && (sleep 10; kill -s 9 -1)'
CMD ["sh", "-c", "/etc/init.d/nginx start && /ServerStatus/server/sergate --config=/ServerStatus/server/config.json --web-dir=/usr/share/nginx/html"]