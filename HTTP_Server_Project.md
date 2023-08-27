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

**-** `mod_rewrite` Use Case: Redirect URLs and rewrite paths.<br>
**-** `mod_ssl` Use Case: Enable HTTPS encryption.<br>
**-** `mod_proxy` Use Case: Proxy requests to backend servers.<br>
**-** `mod_auth_basic` Use Case: Implement basic HTTP authentication.<br>
**-** `mod_headers` Use Case: Modify HTTP headers.<br>
**-** `mod_expires` Use Case: Set content expiration dates.<br>
**-** `mod_security` Use Case: Protect against web attacks.<br>
**-** `mod_cgi` Use Case: Execute CGI scripts for dynamic content.<br>
**-** `mod_userdir` Use Case: Host user-specific content.

###  HTTP Status Codes

HTTP status codes are short messages sent by web servers to browsers, conveying outcomes of requests. They enable communication, error identification (e.g., "404 Not Found"), redirection ("301 Moved Permanently"), caching control ("304 Not Modified"), SEO and user experience improvement, debugging assistance, efficient server communication, and standardized response handling across the web.

- Informational (1xx): These codes indicate that the server is continuing to process the request and hasn't completed it yet. They're usually used for communication purposes rather than indicating success or failure.

- Successful (2xx): These codes indicate that the request was successfully received, understood, and accepted by the server. For example, "200 OK" signifies that the request was successful, and the requested information is included in the response.

- Redirection (3xx): These codes indicate that further action needs to be taken to fulfill the request. They are often used for redirecting the client to a different location.

- Client Errors (4xx): These codes indicate that the client's request has failed due to an error on the client's side. For instance, "404 Not Found" indicates that the requested resource could not be found on the server.

- Server Errors (5xx): These codes indicate that the server failed to fulfill a valid request. They point to an issue on the server's side that prevented it from processing the request.

#### Common HTTP Status Codes

- 200 OK: The request was successful.

- 201 Created: The request was successful, and a new resource was created.

- 204 No Content: The request was successful, but there is no content to send in the response.

- 301 Moved Permanently: The requested resource has been permanently moved to a new location.

- 302 Found (Moved Temporarily): The requested resource has been temporarily moved to a different location.

- 400 Bad Request: The request was malformed or contains invalid parameters.

- 401 Unauthorized: Authentication is required and has failed or has not been provided.

- 403 Forbidden: The server understood the request, but the server refuses to authorize it.

- 404 Not Found: The requested resource was not found on the server.

- 500 Internal Server Error: The server encountered an error while processing the request.