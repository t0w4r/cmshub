FROM ubuntu:16.04
MAINTAINER wh1t3P1g
RUN echo "deb http://mirrors.aliyun.com/ubuntu/ xenial main restricted universe multiverse" > /etc/apt/sources.list
RUN echo "deb http://mirrors.aliyun.com/ubuntu/ xenial-security main restricted universe multiverse" >> /etc/apt/sources.list
RUN echo "deb http://mirrors.aliyun.com/ubuntu/ xenial-updates main restricted universe multiverse" >> /etc/apt/sources.list
RUN echo "deb http://mirrors.aliyun.com/ubuntu/ xenial-backports main restricted universe multiverse" >> /etc/apt/sources.list
RUN apt-get update -y
RUN apt-get upgrade -y
RUN apt-get install -y php
RUN apt-get install -y apache2
RUN apt-get install -y php-xdebug
RUN echo "xdebug.show_error_trace = 1" >> /etc/php/7.0/mods-available/xdebug.ini

EXPOSE 80
EXPOSE 9000