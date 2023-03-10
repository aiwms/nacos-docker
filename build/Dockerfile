FROM openjdk:8-jdk-alpine

# set environment
ENV MODE="standalone" \
    PREFER_HOST_MODE="ip"\
    BASE_DIR="/home/nacos" \
    CLASSPATH=".:/home/nacos/conf:$CLASSPATH" \
    CLUSTER_CONF="/home/nacos/conf/cluster.conf" \
    FUNCTION_MODE="all" \
    NACOS_USER="nacos" \
    JAVA="java" \
    JVM_XMS="1g" \
    JVM_XMX="1g" \
    JVM_XMN="512m" \
    JVM_MS="128m" \
    JVM_MMS="320m" \
    NACOS_DEBUG="n" \
    TOMCAT_ACCESSLOG_ENABLED="false" \
    TIME_ZONE="Asia/Shanghai"

ARG NACOS_VERSION=2.0.3
ARG HOT_FIX_FLAG=""

WORKDIR $BASE_DIR


COPY ["nacos","./"]
ADD bin/docker-startup.sh bin/docker-startup.sh
ADD conf/application.properties conf/application.properties
ADD conf/nacos-logback.xml conf/nacos-logback.xml

RUN apk update && apk add --no-cache libc6-compat
# set startup log dir
RUN mkdir -p logs \
	&& cd logs \
	&& touch start.out \
	&& ln -sf /dev/stdout start.out \
	&& ln -sf /dev/stderr start.out \
    && ln -snf /usr/share/zoneinfo/$TIME_ZONE /etc/localtime && echo $TIME_ZONE > /etc/timezone \
    && ln -s /lib/libc.musl-x86_64.so.1 /lib/ld-linux-x86-64.so.2
RUN chmod +x bin/docker-startup.sh

EXPOSE 8848

ENTRYPOINT ["sh","bin/docker-startup.sh"]
