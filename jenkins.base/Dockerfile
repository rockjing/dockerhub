#FROM jenkins/jenkins:2.202
FROM jenkins/jenkins:2.295

MAINTAINER rock_jing@sina.com

# if we want to install via apt
USER root




#开始安装bash
# 修改为国内镜像源
#RUN cp /etc/apk/repositories /etc/apk/repositories.bak
#RUN echo "http://mirrors.aliyun.com/alpine/v3.4/main/" > /etc/apk/repositories
#apt-get update && sudo apt-get install bash
#RUN   apt-get install --only-upgrade bash
#RUN rm -r xmlstar*
#RUN apt install gcc
RUN mv /etc/apt/sources.list /etc/apt/sources.list.bak
RUN chmod g+r /etc/apt/sources.list.bak
ADD sources.list /etc/apt/sources.list
RUN chmod g+r /etc/apt/sources.list


RUN apt-get update
RUN su jenkins && apt-get install  xmlstarlet tree -y && \
apt-get install  libncursesw5 libtinfo5 libpython2.7-stdlib python2.7   -y --allow-remove-essential

RUN echo alias python2='python2.7' >> ~/.bashrc

#修改时区
ENV TZ=Asia/Shanghai
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

USER jenkins
