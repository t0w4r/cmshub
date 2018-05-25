FROM 0kami/web:apache2_php7

MAINTAINER wh1t3P1g <https://github.com/0kami>

RUN apt-get install -y php-xdebug
RUN echo "zend_extension=xdebug.so" > /etc/php/7.0/mods-available/xdebug.ini \
    && echo "xdebug.idekey=PHPSTORM" >> /etc/php/7.0/mods-available/xdebug.ini \
    && echo "xdebug.remote_enable=on" >> /etc/php/7.0/mods-available/xdebug.ini \
    && echo "xdebug.remote_autostart=off" >> /etc/php/7.0/mods-available/xdebug.ini \
    && echo "xdebug.remote_host=10.254.254.254" >> /etc/php/7.0/mods-available/xdebug.ini \
    && echo "xdebug.remote_port=9001" >> /etc/php/7.0/mods-available/xdebug.ini \
    && echo "xdebug.remote_log=/var/log/apache2/xdebug.log"