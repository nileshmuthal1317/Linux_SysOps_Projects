# Title: Deploying SSH Server for Secure Remote access.

### Description: Explore the implementation of encrypted remote access and secure command-line execution.

Server Side: Install SSH Server 	
```
yum install openssh-server; systemctl restart ssh; systemctl enable sshd
```
Check Firewall for Port or Service and Add Rule if Not Allowed
```
firewall-cmd --list-all | grep -e "ports" -e "services"
```
```
sudo firewall-cmd --add-service=ssh --permanent; sudo firewall-cmd --reload
```
Client Side: Install SSH Client
```
yum install openssh-client;
```
Accessing the server
```
ssh username@remote_host
ssh -p port_number username@remote_host
```

### Configuration for Passwordless (Key-Based) Authentication.

An SSH key pair can be generated or found in the ~/.ssh/ directory.
```
Private Key Location: ~/.ssh/id_rsa
Public Key Location: ~/.ssh/id_rsa.pub
```

**Manual Server Side Method**

- Copy the public key of the client machine and store it in the server user's `~/.ssh/authorized_keys` file.
- This enables access to the server using the key-based method, eliminating the need for a password.


**Alternatively, we can generate the key pair and copy the public key to the remote server.**
- Default encryption is usually RSA, set Passphrase to add layer of security  

Generate an SSH key pair on your local machine
```
ssh-keygen 
```
Copy your public key to the remote server for authentication
```
ssh-copy-id username@remote_host
```
Executing Commands Remotely
```
ssh username@remote_host command
``` 

### Common configuration options in `/etc/ssh/ssh_config`.

`PasswordAuthentication no`
`Port 2222`
`PermitRootLogin yes`