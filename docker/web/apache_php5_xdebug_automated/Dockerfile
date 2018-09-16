FROM 0kami/web:apache2_php5

MAINTAINER wh1t3P1g <https://github.com/0kami>

COPY run.sh /root
WORKDIR /root

RUN apt-get install -y php5-xdebug \
    && echo "zend_extension=xdebug.so" > /etc/php5/mods-available/xdebug.ini \
    && echo "xdebug.idekey=PHPSTORM" >> /etc/php5/mods-available/xdebug.ini \
    && echo "xdebug.remote_enable=on" >> /etc/php5/mods-available/xdebug.ini \
    && echo "xdebug.remote_autostart=off" >> /etc/php5/mods-available/xdebug.ini \
    && echo "xdebug.remote_host=10.254.254.254" >> /etc/php5/mods-available/xdebug.ini \
    && echo "xdebug.remote_port=9001" >> /etc/php5/mods-available/xdebug.ini \
    && echo "xdebug.remote_log=/var/log/apache2/xdebug.log" \
    && chmod a+x run.sh

CMD ["/bin/sh","-c","/root/run.sh"]

# xdebug.remote_host=选择IDE运行的ip
# xdebug.remote_port=选择IDE运行的ip的端口
# Languages & Frameworks > PHP > Debug > DBGp Proxy 设置
# idekey host port
# 