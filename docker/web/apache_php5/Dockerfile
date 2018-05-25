FROM 0kami/base:ubuntu_14_04

MAINTAINER wh1t3P1g <https://github.com/0kami>

RUN apt-get install -y apache2
RUN apt-get install -y php5 php5-common php5-mysql php5-curl php5-gd php5-intl php-pear php5-imagick php5-imap php5-mcrypt php5-memcache php5-ming php5-ps php5-pspell php5-recode php5-snmp php5-sqlite php5-tidy php5-xmlrpc php5-xsl php5-xcache php5-mcrypt

# first start a mysql container
# docker run --name mysqldb -e MYSQL_ROOT_PASSWORD=password -p 3306:3306 -d mysql
# docker run 
#           --name apache2_php5 
#           -it
#           -p 80:80
#           -v /Applications/XAMPP/xamppfiles/htdocs:/var/www/html 
#           -v /tmp/logs/apache2:/var/log/apache2
#           --link mysqldb:mysql
#           --privileged  0kami/web:apache2_php5