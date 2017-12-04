```
yum -y install mariadb mariadb-server httpd php  php-mysql php-gd php-ldap php-mbstring php-odbc php-pear php-xml php-xmlrpc

systemctl enable httpd 
systemctl enable mariadb
systemctl start httpd 
systemctl start mariadb


mysql_secure_installation

开启远程登录:
GRANT ALL PRIVILEGES ON *.* TO root@"%" IDENTIFIED BY "123456";
flush privileges;

echo "Addtype application/x-httpd-php .php .phtml" >> /etc/httpd/httpd.conf  //支持在html中运行php

systemctl restart httpd 

vi /var/www/html/info.php
<?php
phpinfo();
?>




CREATE DATABASE `test` DEFAULT CHARACTER SET utf8 COLLATE utf8_general_ci;
use test;
CREATE TABLE login(id INT(10) UNSIGNED AUTO_INCREMENT PRIMARY KEY,username VARCHAR(30) NOT NULL,password VARCHAR(30) NOT NULL) ENGINE=InnoDB DEFAULT CHARSET=utf8;

insert into login values(7,'tom','123456');

```
