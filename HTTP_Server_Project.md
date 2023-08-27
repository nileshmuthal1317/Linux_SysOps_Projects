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
Test the URL in a local browser `http://server_IP_address/`

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
        
192.168.128.129 www.website1.com
192.168.128.129 www.website2.com
```
Test the URL in a local browser `http://www.website1.com/`
Test the URL in a local browser `http://www.website2.com/`

### Securing a website using a self-signed certificate.

Install `mod_ssl`
```
yum install mod_ssl
```
Generate a New Certificate
```
mkdir /etc/ssl/private
chmod 700 /etc/ssl/private

openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/ssl/private/apache-selfsigned-website1.key -out /etc/ssl/certs/apache-selfsigned-website1.crt

Enter the required details like email, location etc,
```
Update the Virtual Host Configuration File
```
vim /etc/httpd/conf.d/website1.conf

<VirtualHost *:443>
    ServerName www.website1.com
    DocumentRoot /var/www/website1
    SSLEngine on
    SSLCertificateFile /etc/ssl/certs/apache-selfsigned-website1.crt
    SSLCertificateKeyFile /etc/ssl/private/apache-selfsigned-website1.key
</VirtualHost>
```
Creating a Redirect from HTTP to HTTPS
```
vi /etc/httpd/conf.d/non-ssl.conf

<VirtualHost *:80>
       ServerName www.website1.com
        Redirect "/" "https://www.website1.com/"
</VirtualHost>
```
Test the apache configuration and restart apache
```
apachectl configtest; systemctl restart httpd.services
```
Test the Apache configuration and restart Apache
```
http://www.website1.com
```
**Since this is a self-signed certificate, it's expected that a security warning will appear. You might see a padlock with an "x" over it or a triangle with an exclamation point. This simply indicates that the certificate cannot be validated. However, rest assured that your connection is still encrypted.**

### Apache Modules

Apache modules are like plugins for the Apache web server. They add extra features and abilities, such as handling secure connections, managing URLs, or improving performance. Modules are like building blocks that you can add or remove to tailor your web server's behavior to your website's needs.

````
`mod_rewrite` Use Case: Redirect URLs and rewrite paths.
`mod_ssl` Use Case: Enable HTTPS encryption.
`mod_proxy` Use Case: Proxy requests to backend servers.
`mod_auth_basic` Use Case: Implement basic HTTP authentication.
`mod_headers` Use Case: Modify HTTP headers.
`mod_expires` Use Case: Set content expiration dates.
`mod_deflate` Use Case: Compress web content for faster delivery.
`mod_security` Use Case: Protect against web attacks.
`mod_cgi` Use Case: Execute CGI scripts for dynamic content.
`mod_userdir` Use Case: Host user-specific content.
````
