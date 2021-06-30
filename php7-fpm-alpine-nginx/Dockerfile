FROM php:7.3.6-fpm-alpine3.8

ENV TZ=Asia/Shanghai
ENV ALPINE_OS_VERSION=v3.8


# 修改为国内镜像源
RUN cp /etc/apk/repositories /etc/apk/repositories.bak
RUN echo "http://mirrors.aliyun.com/alpine/$ALPINE_OS_VERSION/main/" > /etc/apk/repositories
RUN echo "http://mirrors.aliyun.com/alpine/$ALPINE_OS_VERSION/community/" >> /etc/apk/repositories



#必须安装tzdata后，后面才有  /usr/share/zoneinfo/shanghai
#refer to https://blog.csdn.net/qq_38778137/article/details/84896150
RUN apk update && apk add tzdata && \
    export TZ=Asia/Shanghai && \
    ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone
#
RUN apk update && apk add make && apk add --no-cache --update-cache  gcc g++ autoconf

#Install  mbstring
# https://askubuntu.com/questions/491629/how-to-install-php-mbstring-extension-in-ubuntu
RUN apk add php7-mbstring

#默认启用了，似乎不用enable
#RUN docker-php-ext-enable mbstring

#install pdo_mysql
RUN docker-php-ext-install -j$(nproc) pdo_mysql &&  docker-php-ext-enable pdo_mysql

#install curl
RUN apk add  --update  curl-dev openssl  && docker-php-ext-install curl && \
    apk del gcc g++ &&\
    rm -rf /var/cache/apk/*

#install curl gd
RUN apk  add libpng-dev \
    && docker-php-ext-configure gd --with-freetype --with-jpeg \
    && docker-php-ext-install -j$(nproc) gd \
    && apk del gcc g++ && \
    rm -rf /var/cache/apk/*




#部署目录位置
RUN  mkdir -p /data/logs && mkdir -p /data/logs/php && mkdir -p /data/www
#记录php的信息
RUN php -m >/data/logs/php-modules-list.txt && php -i >> /data/logs/php-info.txt



#指定工作位置
WORKDIR  /data/www


#增加nginx
RUN apk update && apk add nginx && \
    apk add m4 autoconf make gcc g++ linux-headers && \
    mkdir /run/nginx -p && \
    touch /run/nginx/nginx.pid && \
    apk del m4 autoconf make gcc g++ linux-headers


