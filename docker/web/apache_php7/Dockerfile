FROM 0kami/base:ubuntu_16_04

MAINTAINER wh1t3P1g <https://github.com/0kami>

RUN apt-get update -y && apt-get upgrade -y
RUN apt-get install -y php
RUN apt-get install -y apache2
RUN apt-get install -y libapache2-mod-php php-mcrypt php-mysql php-curl php-gd php-xml


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