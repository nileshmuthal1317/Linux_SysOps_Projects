# Title: Deploying Apache Web Server.

### Description: Create a web server to serve web content and handle HTTP requests.

### Apache Installation

Update Package Repositories using 
```
yum update
```
Install Apache
```
yum install httpd; systemctl restart httpd; systemctl enable httpd
```
Ensure Firewall for Port or Service and Add Rule if Not Permitted
```
firewall-cmd --list-all | grep -e "ports" -e "services"
```
```
firewall-cmd --add-service=http --permanent
firewall-cmd --add-service=https --permanent
firewall-cmd --reload
```
Test the URL of any local browser `http://server_IP_address/`

### Apache Virtual Host: Host multiple websites on a single server.

By default CentOS cosider all `.conf` file
Create a new `.conf` file for each website user `/etc/httpd/conf.d/` 
```
vim /etc/httpd/conf.d/website1.conf

<VirtualHost *:80>
    ServerName www.website1.com
    DocumentRoot /var/www/website1
</VirtualHost>

vim /etc/httpd/conf.d/website2.conf

<VirtualHost *:80>
    ServerName www.website2.com
    DocumentRoot /var/www/website2
</VirtualHost>
```
**Upload the website in respective DocumentRoot folders**

Test Configuration and Restart Apache
```
systemctl restart httpd; apachectl configtest
```
**For DNS configuration, update the hosts file on your local system.**
- For Windows `C:\Windows\System32\drivers\etc\hosts`
- For Linux `/etc/hosts`
```
<Server_IP_Address> <The website name should correspond to the configuration in the virtual host's conf file.>
192.168.128.129 www.website.com
192.168.128.129 www.website1.com
192.168.128.129 www.website2.com
```
