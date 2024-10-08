# Apache

# Title: Deploying Apache Web Server.

### Description: Create a web server to serve web content and handle HTTP requests.

### Apache Installation

**Update Package Repositories using**

```bash
yum update
```

**Install Apache**

```bash
yum install httpd; systemctl restart httpd; systemctl enable httpd
```

**Ensure Firewall for Port or Service and Add Rule if Not Permitted**

```bash
firewall-cmd --list-all | grep -e "ports" -e "services"
```

```bash
firewall-cmd --add-service=http --permanent
firewall-cmd --add-service=https --permanent
firewall-cmd --reload
```

**Test the URL in a local browser `http://server_IP_address/`**

### Apache Virtual Host: Host multiple websites on a single server.

By default CentOS cosider all `.conf` file Create a new `.conf` file for each website user `/etc/httpd/conf.d/`

```bash
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

**Upload the website in respective Document Root folders**

Test Configuration and Restart Apache

```bash
systemctl restart httpd; apachectl configtest
```

**For DNS configuration, update the hosts file on your local system.**

- For Windows `C:\Windows\System32\drivers\etc\hosts`
- For Linux `/etc/hosts`

```bash
# <Server_IP_Address> <The website name should correspond to the configuration in the virtual host's conf file.>

192.168.128.129 www.website1.com
192.168.128.129 www.website2.com
```

Test the URL in a local browser `http://www.website1.com/` Test the URL in a local browser `http://www.website2.com/`

### Securing a website using a self-signed certificate.

**Install `mod_ssl`**

```bash
yum install mod_ssl
```

**Generate a New Certificate**

```bash
mkdir /etc/ssl/private
chmod 700 /etc/ssl/private

openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/ssl/private/apache-selfsigned-website1.key -out /etc/ssl/certs/apache-selfsigned-website1.crt

# Enter the required details like email, location etc,
```

**Update the Virtual Host Configuration File**

```bash
vim /etc/httpd/conf.d/website1.conf

<VirtualHost *:443>
    ServerName www.website1.com
    DocumentRoot /var/www/website1
    SSLEngine on
    SSLCertificateFile /etc/ssl/certs/apache-selfsigned-website1.crt
    SSLCertificateKeyFile /etc/ssl/private/apache-selfsigned-website1.key
</VirtualHost>

```

**Creating a Redirect from HTTP to HTTPS**

```bash
vi /etc/httpd/conf.d/non-ssl.conf

<VirtualHost *:80>
       ServerName www.website1.com
        Redirect "/" "https://www.website1.com/"
</VirtualHost>
```

**Test the Apache configuration and restart Apache**

```bash
apachectl configtest; systemctl restart httpd.services
```

**Test the Apache configuration and restart Apache**

```bash
http://www.website1.com
```

**Since this is a self-signed certificate, it's expected that a security warning will appear. You might see a padlock with an "x" over it or a triangle with an exclamation point. This simply indicates that the certificate cannot be validated. However, rest assured that your connection is still encrypted.**

### Apache Modules

Apache modules are like plugins for the Apache web server. They add extra features and abilities, such as handling secure connections, managing URLs, or improving performance. Modules are like building blocks that you can add or remove to tailor your web server's behavior to your website's needs.

-  `mod_rewrite` Use Case: Redirect URLs and rewrite paths.
-  `mod_ssl` Use Case: Enable HTTPS encryption.
-  `mod_proxy` Use Case: Proxy requests to backend servers.
-  `mod_auth_basic` Use Case: Implement basic HTTP authentication.
-  `mod_headers` Use Case: Modify HTTP headers.
-  `mod_expires` Use Case: Set content expiration dates.
-  `mod_security` Use Case: Protect against web attacks.
-  `mod_cgi` Use Case: Execute CGI scripts for dynamic content.
-  `mod_userdir` Use Case: Host user-specific content.

### Logging and troubleshooting Apache on CentOS 8

- `Access Log` This log records all requests made to the Apache server. The default access log file is named access_log. You can find it at `/var/log/httpd/access_log`
- `Error Log` The error log records various errors encountered by the Apache server, including issues with configuration, server errors, and client errors. The default error log file is named error_log. You can find it at `/var/log/httpd/error_log`
- `Virtual Hosts` If you have set up virtual hosts (multiple websites on a single server), each virtual host might have its own access and error log files named after the virtual host configuration. For example, if you have a virtual host named example.com, its access log might be `/var/log/httpd/example.com-access_log`
- We can set custom format for access logs - Also we can set the LogLevel to control the amount of detail that is logged in the error log.

```bash
vim /etc/httpd/conf.d/website1.conf

<VirtualHost *:443>
    ServerName www.website1.com
    DocumentRoot /var/www/website1

    SSLEngine on
    SSLCertificateFile /etc/ssl/certs/apache-selfsigned-website1.crt
    SSLCertificateKeyFile /etc/ssl/private/apache-selfsigned-website1.key

    CustomLog /var/log/httpd/access.log
    ErrorLog /var/log/httpd/error.log

</VirtualHost>
```

### `.htaccess`

The .htaccess file is a distributed configuration file that allows you to modify the configuration for a specific directory, including all its subdirectories. It provides a way to override certain server configuration settings on a per-directory basis without requiring access to the main server configuration files. This can be particularly useful on shared hosting environments where users don't have direct access to the main server configuration.

**Key points about .htaccess**

- It applies to a specific directory and its subdirectories.
- It allows you to control various aspects of how your website works, including authentication, URL rewriting, and access control.
- Changes made in .htaccess files are read and applied on each request, which can impact performance if not used judiciously.
- `.htaccess` files should be used sparingly due to performance considerations, and it's recommended to use the main server configuration whenever possible.

### `/etc/httpd/conf.d/domain_name.com`

/etc/httpd/conf.d/domain_name.com: The /etc/httpd/conf.d/domain_name.com (or a similar path, depending on your system configuration) refers to a location where you can place custom configuration files for individual domains or virtual hosts on your server. These configuration files are typically included from the main Apache configuration and allow you to organize your settings in a modular way.

**Key points about /etc/httpd/conf.d/domain_name.com**

- It's used for creating custom configurations for specific domains or virtual hosts.
- This approach is better suited for global configuration changes related to a particular domain.
- Configuration changes here are generally applied at server startup or reload, so they don't have the performance overhead associated with .htaccess files.

### HTTP Status Codes

HTTP status codes are short messages sent by web servers to browsers, conveying outcomes of requests. They enable communication, error identification (e.g., "404 Not Found"), redirection ("301 Moved Permanently"), caching control ("304 Not Modified"), SEO and user experience improvement, debugging assistance, efficient server communication, and standardized response handling across the web.

- Informational (1xx): These codes indicate that the server is continuing to process the request and hasn't completed it yet. They're usually used for communication purposes rather than indicating success or failure.
- Successful (2xx): These codes indicate that the request was successfully received, understood, and accepted by the server. For example, "200 OK" signifies that the request was successful, and the requested information is included in the response.
- Redirection (3xx): These codes indicate that further action needs to be taken to fulfill the request. They are often used for redirecting the client to a different location.
- Client Errors (4xx): These codes indicate that the client's request has failed due to an error on the client's side. For instance, "404 Not Found" indicates that the requested resource could not be found on the server.
- Server Errors (5xx): These codes indicate that the server failed to fulfill a valid request. They point to an issue on the server's side that prevented it from processing the request.

### Common HTTP Status Codes

-  `200 OK` The request was successful.
-  `201 Created` The request was successful, and a new resource was created.
-  `204 No Content` The request was successful, but there is no content to send in the response.
-  `301 Moved Permanently` The requested resource has been permanently moved to a new location.
-  `302 Found (Moved Temporarily)` The requested resource has been temporarily moved to a different location.
-  `400 Bad Request` The request was malformed or contains invalid parameters.
-  `401 Unauthorized` Authentication is required and has failed or has not been provided.
-  `403 Forbidden` The server understood the request, but the server refuses to authorize it.
-  `404 Not Found` The requested resource was not found on the server.
-  `500 Internal Server Error` The server encountered an error while processing the request.
