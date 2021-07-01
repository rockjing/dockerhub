FROM nimmis/alpine-java:oracle-8-jdk

ENV TZ=Asia/Shanghai
ENV ALPINE_OS_VERSION=v3.4
ENV GLIBC_VERSION=2.30-r0
#ENV PUB_URL=https://alpine-pkgs.sgerrand.com/sgerrand.rsa.pub
ENV PUB_URL=http://cdn-media-dev.umu360.com/jdk1.8_build/sgerrand.rsa.pub
#ENV GLIBc_URL=https://github.com/sgerrand/alpine-pkg-glibc/releases/download/2.30-r0/glibc-2.30-r0.apk
ENV GLIBC_URL=http://cdn-media-dev.umu360.com/jdk1.8_build/glibc-$GLIBC_VERSION.apk
ENV GLIBC_BIN_URL=http://cdn-media-dev.umu360.com/jdk1.8_build/glibc-bin-$GLIBC_VERSION.apk
ENV GLIBC_DEV_URL=http://cdn-media-dev.umu360.com/jdk1.8_build/glibc-dev-$GLIBC_VERSION.apk
ENV GLIBC_I18N_URL=http://cdn-media-dev.umu360.com/jdk1.8_build/glibc-i18n-$GLIBC_VERSION.apk


# 修改为国内镜像源
RUN cp /etc/apk/repositories /etc/apk/repositories.bak
RUN echo "http://mirrors.aliyun.com/alpine/$ALPINE_OS_VERSION/main/" > /etc/apk/repositories
RUN echo "http://mirrors.aliyun.com/alpine/$ALPINE_OS_VERSION/community/" >> /etc/apk/repositories

#必须安装tzdata后，后面才有  /usr/share/zoneinfo/shanghai
#refer to https://blog.csdn.net/qq_38778137/article/details/84896150
RUN apk update && apk add tzdata && \
    export TZ=Asia/Shanghai && \
    ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone


# Install language pack
RUN apk --no-cache add ca-certificates \
&& wget -q -O /etc/apk/keys/sgerrand.rsa.pub $PUB_URL \
&& wget $GLIBC_URL \
&& wget $GLIBC_BIN_URL \
&& wget $GLIBC_I18N_URL

RUN apk add glibc-bin glibc

#UN apk add glibc-bin-$GLIBC_VERSION.apk
RUN apk add  glibc-i18n-$GLIBC_VERSION.apk
RUN rm -rf *.apk && rm -rf /var/cache/apk/*




# Iterate through all locale and install it
# Note that locale -a is not available in alpine linux, use `/usr/glibc-compat/bin/locale -a` instead
COPY ./locale.MD locale.MD
RUN cat locale.MD | xargs -i /usr/glibc-compat/bin/localedef -i {} -f UTF-8 {}.UTF-8
#RUN /usr/glibc-compat/bin/localedef -i en_US -f UTF-8 en_US.UTF-8

# Set the lang, you can also specify it as as environment variable through docker-compose.yml
ENV LANG=en_US.UTF-8
ENV LANGUAGE=en_US.UTF-8



