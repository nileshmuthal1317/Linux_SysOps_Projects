# Title: Deploying MySQL Database Server.

### Description: Create a web server to serve web content and handle HTTP requests.

### Installing MySQL
```
yum install mysql-server;
```
```
systemctl start mysqld.service; systemctl enable mysqld.service; systemctl status mysqld
```
### Securing MySQL
Use below command to configure the security credencial for the root account
```
mysql_secure_installation
```
### Testing MySQL
You can verify your installation and get information about it by connecting with the mysqladmin tool
```
 mysqladmin -u root -p version
```
Connect to MySQL
```
mysql -u root -p
```
### Working with MySQL Databases
```
CREATE DATABASE test_database;
SHOW DATABASES;
```
### Installing PHP
```
yum install php php-mysqlnd
systemctl restart httpd

vim /var/www/html/info.php

<?php
phpinfo();
```
### Install phpMyAdmin
```
yum install install -y https://rpms.remirepo.net/enterprise/remi-release-8.rpm
yum module reset php
yum module enable php:remi-7.4
yum install phpmyadmin
```

wget https://files.phpmyadmin.net/phpMyAdmin/5.2.1/phpMyAdmin-5.2.1-all-languages.zip
	