FROM alpine:latest

ENV OS_VERSION=v3.10
ENV GLIBC_VERSION=2.30-r0
#ENV PUB_URL=https://alpine-pkgs.sgerrand.com/sgerrand.rsa.pub
ENV PUB_URL=http://cdn-media-dev.umu360.com/jdk1.8_build/sgerrand.rsa.pub
#ENV GLIBc_URL=https://github.com/sgerrand/alpine-pkg-glibc/releases/download/2.30-r0/glibc-2.30-r0.apk
ENV GLIBC_URL=http://cdn-media-dev.umu360.com/jdk1.8_build/glibc-$GLIBC_VERSION.apk
ENV GLIBC_BIN_URL=http://cdn-media-dev.umu360.com/jdk1.8_build/glibc-bin-$GLIBC_VERSION.apk
ENV GLIBC_DEV_URL=http://cdn-media-dev.umu360.com/jdk1.8_build/glibc-dev-$GLIBC_VERSION.apk
ENV GLIBC_I18N_URL=http://cdn-media-dev.umu360.com/jdk1.8_build/glibc-i18n-$GLIBC_VERSION.apk

ENV JDK8_FILE=jdk-8u291-linux-x64.tar.gz
ENV JDK8_URL=http://cdn-media-dev.umu360.com/jdk1.8_build/jdk-8u291-linux-x64.tar.gz
ENV JDK8_DIR=jdk1.8.0_291
#出现乱码，比较了环境参数增加这个参数




## 修改为国内镜像源
#RUN cp /etc/apk/repositories /etc/apk/repositories.bak
RUN echo "http://mirrors.aliyun.com/alpine/$OS_VERSION/main/" > /etc/apk/repositories && \
    echo "http://mirrors.aliyun.com/alpine/$OS_VERSION/community/" >> /etc/apk/repositories


RUN apk --no-cache add ca-certificates \
&& wget -q -O /etc/apk/keys/sgerrand.rsa.pub $PUB_URL \
&& wget $GLIBC_URL \
&& wget $GLIBC_BIN_URL \
&& wget $GLIBC_DEV_URL \
&& wget $GLIBC_I18N_URL \
&& apk add glibc-$GLIBC_VERSION.apk \
&& apk add glibc-bin-$GLIBC_VERSION.apk \
&& apk add glibc-dev-$GLIBC_VERSION.apk \
&& apk add glibc-i18n-$GLIBC_VERSION.apk \
&& apk add libgcc \
&& apk add tree \
&& rm -rf *.apk \
&& rm -rf /var/cache/apk/*

RUN export PATH=/usr/glibc-compat/bin:$PATH
RUN localedef -i en_US -f UTF-8 en_US.UTF-8 C.UTF-8
ENV LANG=C.UTF-8

RUN wget $JDK8_URL
RUN tar -zxvf $JDK8_FILE -C /usr/local && rm -f $JDK8_FILE
RUN cd /usr/local/$JDK8_DIR && \
rm -rf COPYRIGHT LICENSE README release THIRDPARTYLICENSEREADME-JAVAFX.txt THIRDPARTYLICENSEREADME.txt Welcome.html && \
#rm -rf lib/plugin.jar \
#lib/ext/jfxrt.jar \
#bin/javaws \
#lib/javaws.jar \
#lib/desktop \
#plugin \
#lib/deploy* \
#lib/*javafx* \
#lib/*jfx* \
#lib/amd64/libdecora_sse.so \
#lib/amd64/libprism_*.so \
#lib/amd64/libfxplugins.so \
#lib/amd64/libglass.so \
#lib/amd64/libgstreamer-lite.so \
#lib/amd64/libjavafx*.so \
#lib/amd64/libjfx*.so \
rm -rf man \
legal \
*src.zip


RUN cd /usr/local/$JDK8_DIR && tree



# 设置JAVA变量环境
ENV JAVA_HOME=/usr/local/$JDK8_DIR
ENV CLASSPATH=$JAVA_HOME/bin
ENV PATH=.:$JAVA_HOME/bin:$PATH
#CMD ["java","-version"]


# RUN echo $JAVA_HOME && cd $JAVA_HOME && ls -l
#RUN java -version

#解决时区问题
ENV TZ=Asia/Shanghai
ENV ALPINE_OS_VERSION=v3.4

#COPY newrelic_jar /usr/local/newrelic.jar
#COPY newrelic.yml /usr/local/newrelic.yml

# 修改为国内镜像源
RUN cp /etc/apk/repositories /etc/apk/repositories.bak
RUN echo "http://mirrors.aliyun.com/alpine/$ALPINE_OS_VERSION/main/" > /etc/apk/repositories
RUN echo "http://mirrors.aliyun.com/alpine/$ALPINE_OS_VERSION/community/" >> /etc/apk/repositories



#必须安装tzdata后，后面才有  /usr/share/zoneinfo/shanghai
#refer to https://blog.csdn.net/qq_38778137/article/details/84896150
RUN apk update && apk add tzdata && \
    export TZ=Asia/Shanghai && \
    ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone