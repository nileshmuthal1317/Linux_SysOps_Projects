yum install nginx

firewall-cmd --permanent --add-port={80/tcp,443/tcp}
firewall-cmd --reload

systemctl enable nginx

systemctl start nginx

yum list installed nginx

firewall-cmd --list-ports

systemctl is-enabled nginx

Edit the /etc/nginx/nginx.conf file:

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

mkdir -p /var/www/example.com/
mkdir -p /var/www/example.net/

semanage fcontext -a -t httpd_sys_content_t "/var/www/example.com(/.*)?"
restorecon -Rv /var/www/example.com/
semanage fcontext -a -t httpd_sys_content_t "/var/www/example.net(/.\*)?"
restorecon -Rv /var/www/example.net/

systemctl restart nginx

echo "Content for example.com" > /var/www/example.com/index.html
echo "Content for example.net" > /var/www/example.net/index.html
echo "Catch All content" > /usr/share/nginx/html/index.html

Browse the URL

Adding TLS encryption to an NGINX web server


mkdir /etc/pki/CA
cd /etc/pki/CA 
mkdir newcerts certs crl private requests
touch index.txt
echo '1234' > serial


openssl genrsa -out /etc/pki/CA/private/root.ca.key.pem 4096
openssl req  -key /etc/pki/CA/private/root.ca.key.pem -new -x509 -days 7300 -extensions v3_ca -out root.ca.crt.pem

cp root.ca.crt.pem /etc/pki/ca-trust/source/anchors/
update-ca-trust extract

openssl genrsa -out net.key.pem 2048
openssl req -config /etc/pki/tls/openssl.cnf -key net.key.pem -new -out net.csr.pem
openssl ca -config /etc/pki/tls/openssl.cnf -extensions v3_req -days 3650 -in net.csr.pem -out net.crt.pem -cert root.ca.crt.pem

rm net.csr.pem
mv net.crt.pem /etc/pki/CA/certs
mv net.key.pem /etc/pki/CA/private

chmod -R 600 /etc/pki/CA

vi /etc/nginx/nginx.conf

Update below configuration

 server {
        listen       443 ssl;
        server_name  example.net;
        root         /var/www/example.net/;
        ssl_certificate         /etc/pki/CA/certs/net.crt.pem;
        ssl_certificate_key     /etc/pki/CA/private/net.key.pem;
        access_log   /var/log/nginx/example.net/access.log;
        error_log    /var/log/nginx/example.net/error.log;
     }

vi /etc/ssl/openssl.cnf

dir             = /etc/pki/CA
certificate     = $dir/root.ca.crt.pem
private_key     = $dir/private/root.ca.key.pem


Download the root.ca.crt.pem and net.crt.pem rename the extension pen to crt
install the root crt in root certified autherity
you might get the non secure warning cause this is the self signed certicate