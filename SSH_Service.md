# SSH

# Title: Deploying SSH Server for Secure Remote access.

### Description: Explore the implementation of encrypted remote access and secure command-line execution.

**Server Side: Install SSH Server**

```bash
**yum install openssh-server; systemctl restart ssh; systemctl enable sshd**
```

**Ensure Firewall for Port or Service and Add Rule if Not Permitted**

```bash
**firewall-cmd --list-all | grep -e "ports" -e "services"**
```

```bash
**sudo firewall-cmd --add-service=ssh --permanent; sudo firewall-cmd --reload**
```

Client Side: Install SSH Client

```bash
**yum install openssh-client;**
```

Accessing the server

```bash
**ssh username@remote_host
ssh -p port_number username@remote_host**
```

### Configuration for Passwordless (Key-Based) Authentication.

An SSH key pair can be generated or found in the ~/.ssh/ directory.

```bash
**Private Key Location: ~/.ssh/id_rsa
Public Key Location: ~/.ssh/id_rsa.pub**
```

**Manual Server Side Method**

- Copy the public key of the client machine and store it in the server user's `~/.ssh/authorized_keys` file.
- This enables access to the server using the key-based method, eliminating the need for a password.

**Alternatively, we can generate the key pair and copy the public key to the remote server.**

- Default encryption is usually RSA, set Passphrase to add layer of security

**Generate an SSH key pair on your local machine**

```bash
**ssh-keygen**
```

**Copy your public key to the remote server for authentication**

```bash
**ssh-copy-id username@remote_host**
```

**Executing Commands Remotely**

```bash
**ssh username@remote_host command**
```

### Common configuration options in `/etc/ssh/ssh_config`.

**`PasswordAuthentication no Port 2222 PermitRootLogin yes`**

## **SSH Secure Shell**

**"Architect and Functionality"**

**WHY SSH is Used for Secure Remote Server Access**

SSH is utilized for securely accessing remote servers. It provides an encryption mechanism to secure the communication between the client and the remote server.

**HOW IT WORKS**

After installing the SSH server and SSH client on the server and client machine, respectively.

Ensure that the firewall allows inbound traffic over port 22 or any other port configured for SSH communication.

Remote server access can be achieved using two authentication mechanisms: password-based and key-based.

For password-based authentication, simply use ssh user@remote_server and enter your password.

For key-based authentication, you need to generate public and private keys, copy the public key to the server, and disable password-based authentication. Refer to the steps below.

On the server, verify that /username/.ssh/authorized_keys has been copied.

**HOW Public-Private Encryption Works**

Once the public key is copied to the remote server's home directory, a server host key gets saved in the known_hosts file on the client’s end which is used to check identity.

Users can request the establishment of a secure remote session.

The server responds by encrypting the session using the client's public key, which can only be decrypted using the client's private key.

Once this authentication is established, the client can securely access the server without any security concerns.

**What We Achieved**

SSH, the Secure Shell, ensures the security and encryption of traffic between the client machine and the server, enabling secure remote server access, especially crucial for sensitive data and system administration tasks

**SSH Cheat Sheet**

**`yum install openssh-server; systemctl restart ssh; systemctl enable sshd`**

**`firewall-cmd --list-all | grep -e "ports" -e "services"`**

**`firewall-cmd --add-service=ssh --permanent; sudo firewall-cmd –reload`**

**`yum install openssh-client;`**

**`ssh username@remote_host`**

**`ssh -p port_number username@remote_host`**

**`ssh-keygen`**

**`ssh-copy-id username@remote_host`**

**`ssh username@remote_host command`**

**Configuration and Logging**

**`SSH Configuration File /etc/ssh/sshd_config`**

**`Server Side ~/.ssh/authorized_keys`**

**`Private Key Location: ~/.ssh/id_rsa`**

**`Public Key Location: ~/.ssh/id_rsa.pub`**

**`Client Side ~/.ssh/known_hosts`**

**`~/.bashrc and ~/.bash_profile >> User specific shell configurations`**

**`/etc/skel/.bashrc >> Defining user-specific aliases, customizing the shell prompt, enabling or disabling certain shell features, and running commands specific to an interactive session.`**

**`/etc/skel/.bash_profile >> Setting environment variables, configuring the shell prompt, defining global aliases, and running login-time scripts.`**

**`/var/log/secure`**