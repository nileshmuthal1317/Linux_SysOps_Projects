# Databases

# Title: Deploying MySQL Database Server, php, phpMyAdmin.

### Description: Create a web server to serve web content and handle HTTP requests.

### Installing MySQL

```bash
yum install mysql-server;
```

```bash
systemctl start mysqld.service; systemctl enable mysqld.service; systemctl status mysqld
```

### Securing MySQL

Utilize the following command to configure security credentials for the root account.

```bash
mysql_secure_installation
```

### Testing MySQL

You can verify your installation and retrieve information about it by connecting using the `mysqladmin` tool.

```bash
 mysqladmin -u root -p version
```

**Establish a connection to MySQL**

```bash
mysql -u root -p
```

### Working with MySQL Databases

**Managing Databases and Tables**

```bash
CREATE DATABASE test_database;
SHOW DATABASES;
DROP DATABASE test_database;
USE test_database;
```

```bash
CREATE TABLE employees (id INT AUTO_INCREMENT PRIMARY KEY,first_name VARCHAR(50),last_name VARCHAR(50),age INT);
INSERT INTO employees (first_name, last_name, age)VALUES('John', 'Doe', 30),('Jane', 'Smith', 25);
SELECT * FROM employees;
DESCRIBE employees;
SHOW TABLES;
RENAME TABLE old_table_name TO new_table_name;
DROP TABLE table_name;
```

**Managing Users and Permissions**

```bash
CREATE USER 'myuser'@'localhost' IDENTIFIED BY 'Password@007';
SELECT user, host FROM mysql.user;
GRANT ALL PRIVILEGES ON mydatabase.* TO 'myuser'@'localhost';
FLUSH PRIVILEGES;
SHOW GRANTS FOR 'myuser'@'localhost';
```

**Backing Up and Restoring**

```bash
mysqldump -u username -p database_name > backup.sql
mysql -u username -p database_name < backup.sql
```

**Monitoring and Diagnostics**

```bash
SHOW STATUS;
SHOW PROCESSLIST;
SHOW VARIABLES LIKE 'long_query_time';
SHOW VARIABLES LIKE 'slow_query_log';
```

### Installing PHP

```bash
yum install php php-cli php-json php-gd php-mbstring php-pdo php-xml php-mysqlnd

systemctl restart httpd

vim /var/www/html/info.php

<?php
phpinfo();
```

### Install phpMyAdmin

```bash
cd /var/www/
wget <https://files.phpmyadmin.net/phpMyAdmin/5.0.0/phpMyAdmin-5.0.0-all-languages.zip>
unzip phpMyAdmin-5.0.0-all-languages.zip; rm phpMyAdmin-5.0.0-all-languages.zip
mv phpMyAdmin-5.0.0-all-languages phpMyAdmin

```

```bash
chown -Rf apache:apache /var/www/phpMyAdmin
semanage fcontext -a -t httpd_sys_rw_content_t "/var/www/phpMyAdmin(/.*)?"
restorecon -Rv /var/www/phpMyAdmin
```

```bash
vim /etc/httpd/conf.d/phpmyadmin.conf

Alias /phpmyadmin /var/www/phpMyAdmin
```

```bash
systemctl restart httpd
```

Test `phpMyAdmin` by accessing it through the URL `http://server_IP_address/phpmyadmin`, and then log in using the credentials of the root user or a regular user.

### MySQL Logging and Configuration

**Configuration**

- Performance Optimization: Configure MySQL settings to boost database performance and resource efficiency.
- Security: Set up authentication, access controls, and encryption to safeguard your data.
- Resource Management: Allocate system resources effectively to prevent bottlenecks.
- Character Set and Collation: Ensure proper handling of diverse text data for different languages.
- Replication and High Availability: Configure replication for data redundancy and failover.
- Storage Engine Selection: Choose the right storage engine based on your use case.
- Query Optimization: Fine-tune settings for faster query execution.

**Loggings**

- Troubleshooting: Logs help diagnose errors, identify problematic queries, and pinpoint slow performance.
- Security Auditing: Monitor and audit access, detecting unauthorized activity.
- Performance Analysis: Analyze logs to find performance bottlenecks and optimize queries.
- Capacity Planning: Monitor resource utilization for effective capacity planning.
- Compliance and Regulations: Maintain logs to meet industry compliance requirements.
- Replication Monitoring: Logs track replication status, ensuring data consistency.
- Backup Verification: Logs verify the integrity of database backups.

**Configuration file `/etc/my.cnf` or `/etc/my.ini`**

**Database location and Character Set**

```bash
**character-set-server = utf8mb4
collation-server = utf8mb4_unicode_ci
port = 3306
datadir = /var/lib/mysql**
```

**Logging**

```bash
log-error = /var/log/mysql/error.log
general-log = 0
general_log_file = /var/log/mysql/general.log
slow_query_log = 1
slow_query_log_file = /var/log/mysql/slow.log
long_query_time = 2
```

**Performance**

```bash
query_cache_type = 0
query_cache_size = 0
max_connections = 100
key_buffer_size = 64M
max_allowed_packet = 16M
```
