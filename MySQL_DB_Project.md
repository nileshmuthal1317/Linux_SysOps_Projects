# Title: Deploying MySQL Database Server, php, phpMyAdmin.

### Description: Create a web server to serve web content and handle HTTP requests.

### Installing MySQL
```
yum install mysql-server;
```
```
systemctl start mysqld.service; systemctl enable mysqld.service; systemctl status mysqld
```
### Securing MySQL
Utilize the following command to configure security credentials for the root account.
```
mysql_secure_installation
```
### Testing MySQL
You can verify your installation and retrieve information about it by connecting using the `mysqladmin` tool.
```
 mysqladmin -u root -p version
```
Establish a connection to MySQL
```
mysql -u root -p
```
### Working with MySQL Databases
Managing Databases and Tables
```
CREATE DATABASE test_database; 
SHOW DATABASES;
DROP DATABASE test_database;
USE test_database;
```
```
CREATE TABLE employees (id INT AUTO_INCREMENT PRIMARY KEY,first_name VARCHAR(50),last_name VARCHAR(50),age INT);
INSERT INTO employees (first_name, last_name, age)VALUES('John', 'Doe', 30),('Jane', 'Smith', 25);
SELECT * FROM employees;
DESCRIBE employees;
SHOW TABLES;
RENAME TABLE old_table_name TO new_table_name;
DROP TABLE table_name;
```
Managing Users and Permissions
```
CREATE USER 'myuser'@'localhost' IDENTIFIED BY 'Password@007';
GRANT ALL PRIVILEGES ON mydatabase.* TO 'myuser'@'localhost';
FLUSH PRIVILEGES;
SELECT user, host FROM mysql.user;
```
Backing Up and Restoring
```
mysqldump -u username -p database_name > backup.sql
mysql -u username -p database_name < backup.sql
```
Monitoring and Diagnostics
```
SHOW STATUS;
SHOW PROCESSLIST;
SHOW VARIABLES LIKE 'long_query_time';
SHOW VARIABLES LIKE 'slow_query_log';
```
General Maintenance
```
FLUSH QUERY CACHE;
FLUSH LOGS;
```
### Installing PHP
```
yum install php php-cli php-json php-gd php-mbstring php-pdo php-xml php-mysqlnd

systemctl restart httpd

vim /var/www/html/info.php

<?php
phpinfo();
```
### Install phpMyAdmin
```
cd /var/www/
wget https://files.phpmyadmin.net/phpMyAdmin/5.0.0/phpMyAdmin-5.0.0-all-languages.zip
unzip phpMyAdmin-5.0.0-all-languages.zip; rm phpMyAdmin-5.0.0-all-languages.zip
mv phpMyAdmin-5.0.0-all-languages phpMyAdmin
```
```
chown -Rf apache:apache /var/www/phpMyAdmin
semanage fcontext -a -t httpd_sys_rw_content_t "/var/www/phpMyAdmin(/.*)?"
restorecon -Rv /var/www/phpMyAdmin
```
```
vim /etc/httpd/conf.d/phpmyadmin.conf

Alias /phpmyadmin /var/www/phpMyAdmin
```
```
systemctl restart httpd
```
Test `phpMyAdmin` by accessing it through the URL `http://server_IP_address/phpmyadmin`, and then log in using the credentials of the root user or a regular user.