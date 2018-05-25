FROM ubuntu:14.04
MAINTAINER wh1t3P1g
RUN echo "deb http://mirrors.aliyun.com/ubuntu/ trusty main restricted universe multiverse" > /etc/apt/sources.list
RUN echo "deb http://mirrors.aliyun.com/ubuntu/ trusty-updates main restricted universe multiverse" >> /etc/apt/sources.list
RUN echo "deb http://mirrors.aliyun.com/ubuntu/ trusty-proposed main restricted universe multiverse" >> /etc/apt/sources.list
RUN echo "deb http://mirrors.aliyun.com/ubuntu/ trusty-backports main restricted universe multiverse" >> /etc/apt/sources.list
RUN apt-get update -y
RUN apt-get upgrade -y
RUN apt-get install -y php5
RUN apt-get install -y apache2
RUN apt-get install -y php5-xdebug
RUN apt-get install -y php5-mysql
RUN apt-get install -y php5-dev
RUN apt-get install -y libmysqlclient15-dev
ENV PHP_PDO_SHARED=1
RUN pecl install pdo
RUN a2enmod rewrite
RUN echo "xdebug.idekey=PHPSTORM" >> /etc/php5/mods-available/xdebug.ini
RUN echo "xdebug.remote_enable = 1" >> /etc/php5/mods-available/xdebug.ini
RUN echo "xdebug.remote_host=172.17.0.1" >> /etc/php5/mods-available/xdebug.ini
RUN echo "xdebug.remote_post = 9000" >> /etc/php5/mods-available/xdebug.ini
RUN echo "xdebug.show_error_trace = 1" >> /etc/php5/mods-available/xdebug.ini

EXPOSE 80
EXPOSE 9000