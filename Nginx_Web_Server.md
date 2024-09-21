# Nginx

# Title: Deploying NGINX Web Server.

### Description: Create a web server to serve web content and handle HTTP requests.

### NGINX Installation

```bash
yum install nginx
```

```bash
firewall-cmd --permanent --add-port={80/tcp,443/tcp}
firewall-cmd --reload
```

```bash
systemctl enable nginx
systemctl start nginx
```

```bash
yum list installed nginx
firewall-cmd --list-ports
systemctl is-enabled nginx
```

### Virtual Host: Host multiple websites on a single server.

**Edit the /etc/nginx/nginx.conf file**

```bash
server {
    listen       80 default_server;
    listen       [::]:80 default_server;
    server_name  _;
    root         /usr/share/nginx/html;
}

server {
    server_name  example.com;
    root         /var/www/example.com/;
    access_log   /var/log/nginx/example.com/access.log;
    error_log    /var/log/nginx/example.com/error.log;
}

server {
    server_name  example.net;
    root         /var/www/example.net/;
    access_log   /var/log/nginx/example.net/access.log;
    error_log    /var/log/nginx/example.net/error.log;
}
```

```bash
mkdir -p /var/www/example.com/
mkdir -p /var/www/example.net/
```

```bash
semanage fcontext -a -t httpd_sys_content_t "/var/www/example.com(/.*)?"
restorecon -Rv /var/www/example.com/
semanage fcontext -a -t httpd_sys_content_t "/var/www/example.net(/.\\*)?"
restorecon -Rv /var/www/example.net/
systemctl restart nginx
```

```bash
echo "Content for example.com" > /var/www/example.com/index.html
echo "Content for example.net" > /var/www/example.net/index.html
echo "Catch All content" > /usr/share/nginx/html/index.html
```

**update the /etc/hosts file for local machine and Browse the URL `http://example.com` and `http://example.net`**

```bash
Server_IP example.com
Server_IP example.net
```

### Adding TLS encryption to an NGINX web server

```bash
mkdir /etc/pki/CA
cd /etc/pki/CA
mkdir newcerts certs crl private requests
touch index.txt
echo '1234' > serial
```

```bash
openssl genrsa -out /etc/pki/CA/private/root.ca.key.pem 4096
openssl req  -key /etc/pki/CA/private/root.ca.key.pem -new -x509 -days 7300 -extensions v3_ca -out root.ca.crt.pem
```

```bash
cp root.ca.crt.pem /etc/pki/ca-trust/source/anchors/
update-ca-trust extract
```

```bash
openssl genrsa -out net.key.pem 2048
openssl req -config /etc/pki/tls/openssl.cnf -key net.key.pem -new -out net.csr.pem
openssl ca -config /etc/pki/tls/openssl.cnf -extensions v3_req -days 3650 -in net.csr.pem -out net.crt.pem -cert root.ca.crt.pem
```

```bash
rm net.csr.pem
mv net.crt.pem /etc/pki/CA/certs
mv net.key.pem /etc/pki/CA/private
chmod -R 600 /etc/pki/CA
```

**Edit `/etc/nginx/nginx.conf` and update below configuration**

```bash
server {
       listen       443 ssl;
       server_name  example.net;
       root         /var/www/example.net/;
       ssl_certificate         /etc/pki/CA/certs/net.crt.pem;
       ssl_certificate_key     /etc/pki/CA/private/net.key.pem;
       access_log   /var/log/nginx/example.net/access.log;
       error_log    /var/log/nginx/example.net/error.log;
    }
```

**Edit `/etc/ssl/openssl.cnf` and update the configuration as below**

```bash
dir             = /etc/pki/CA
certificate     = $dir/root.ca.crt.pem
private_key     = $dir/private/root.ca.key.pem
```

- Download the "root.ca.crt.pem" and "net.crt.pem" files. After downloading them, rename the file extensions from ".pem" to ".crt."
- Install the "root.crt" certificate into the trusted root certificate authority store on your system. This step will depend on your operating system and configuration. Usually, you can do this by importing the certificate into the system's trusted certificate store.
- Once you have installed the root CA, you can verify the certificate path section of the server's digital certificate. This will confirm that the server's certificate is signed by the trusted root CA, establishing the trust chain for secure communication.
- Keep in mind that you might encounter non-secure warnings when using these certificates because they are self-signed certificates. This means they are not issued by a trusted third-party certificate authority. You may need to acknowledge or accept these warnings when using the certificates.
