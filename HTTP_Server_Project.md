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