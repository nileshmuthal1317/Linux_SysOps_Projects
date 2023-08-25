# Deploying SSH Server and Client for remote access

Title: Deploying SSH Server for Secure Remote access  
Description: Explore the implementation of encrypted remote access and secure command-line execution.

## Server Side Configurations

Install OPENSSH-SERVER 	
```
yum install openssh-server; systemctl restart ssh; systemctl enable sshd
```

Check the firewall for port or service if not allowed add the rule in firewall
```
firewall-cmd --list-all | grep -e "ports" -e "services"
```
```
sudo firewall-cmd --add-service=ssh --permanent; sudo firewall-cmd --reload
```

**Configuration for passwordless(key-based) authentication and disabling the password based authentication**


## Client side Configurations

Install OPENSSH-CLIENT
```
yum install openssh-client;
```

SSH into the remote server using the password
```
ssh remote_user@remote_server_IP
```

SSH into remote server using password-less(key-based) authentication
```
ssh-keygen
```

Store the public key to the remote server
```
ssh-copyid
```


