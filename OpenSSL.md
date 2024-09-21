# OpenSSL

**Architect and Operation**

Why Use OpenSSL for Generating Let's Encrypt Self-Signed Certificates?

OpenSSL is a command-line tool used to generate self-signed certificates; a process related to SSL/TLS certificates. Similar to SSH, it provides an encryption mechanism using SSL/TLS certificates to verify the server's authenticity and establish a secure session between the client and the server. T hese certificates can be employed to secure websites, email servers, VPNs, secure FTP, database servers, application servers, RDP, SSH, and more. Certificate Authorities (CAs) issue SSL/TLS certificates after verifying Certificate Signing Requests (CSRs) using a chain of trust.

**How It Works**

There are two methods to obtain SSL/TLS certificates. The first method is to use self-signed certificates generated with OpenSSL, and the second is to obtain them from a Certificate Authority. A limitation of self-signed certificates is that web browsers may not trust them, leading to non-secure warnings. However, the connection remains secure, even if the browser can't verify it. Using a CA certificate has the advantage of the "chain of trust," which ensures web browsers trust the certificate, resulting in the display of a green lock and a secure connection.

To obtain a CA certificate, you can refer to the CA's specific documentation. This explanation focuses on generating CSR requests and installing SSL/TLS certificates for a website using a self-signed certificate. Be sure to configure your web server, deploy the website, link the domain name, and configure web server settings.

**Here are the steps:**

Create directories to store your certificates and update the permissions to secure them.

Begin by configuring the Root CA.

Create a key for the Root CA.

Use the key to create the Root CA certificate.

Create a key for your website.

Using the OpenSSL configuration and the website key, raise the CSR request. Provide the necessary details for identification.

Use the generated CSR file, OpenSSL configuration, and Root CA certificate to generate the SSL/TLS certificate for your website.

Finally, place the website key and certificate in the website's configuration.

Since it's a self-signed certificate, you can download and install the Root CA certificate on Windows machines to establish trust in websites using the same Root CA.

**How do websites ensure your data is safe and private when you browse the internet?**

🌐 Step 1: Enabling SSL/TLS

A website is SSL/TLS enabled, which means it's equipped to establish encrypted sessions with users.

👋 Step 2: Initiating the Connection

When you visit the site, your browser requests a secure connection, signalling, "Hey, let's establish an encrypted session."

🔒 Step 3: Server Responds with Its Certificate

The server responds, "Sure, here is my certificate along with my public key."

🔑 Step 4: Creating the Session Key

You, the client, use the provided public key to create a session key—a unique "secret language" for your conversation.

🔐 Step 5: Decryption with Private Key

The server, using its private key, decrypts the session key you've shared. Now both sides possess the same session key.

🚀 Step 6: Secure Data Exchange

Throughout the session, your messages are encrypted using the shared session key—like speaking in code only both parties understand.

🔒 Step 7: End of Session

Once the session ends, the session key is discarded, ensuring security for future interactions.

**Actual Flow**

**Generate Key Pair**: You start by creating a pair of keys—a private key and a public key.

Think of the private key as a secret key that only you know, and the public key as a key you can freely share.

**Create CSR**: You use your private key to create a special message called a Certificate Signing Request (CSR).

This message includes information about you or your organization (like your domain name) and your public key.

**Submit CSR to Certificate Authority (CA):** You send the CSR to a Certificate Authority, which is like a trusted referee on the internet.

The CA checks the information in your CSR and verifies that you own the domain.

**CA Issues Certificate:** If everything checks out, the CA issues a digital certificate.

This certificate says, "Hey, this public key belongs to the person or organization mentioned in the CSR."

**Install Certificate on Server:** You take the issued certificate and install it on your server along with the private key.

Now, when someone wants to communicate with your server securely, they use the public key from the certificate to encrypt information, and your server uses the private key to decrypt it.

In essence, the private key is like the secret key to your lock, and the public key is like the lock that everyone can use to send you secure messages. The CSR is the formal request you send to a trusted authority to vouch for you and say, "Yes, this person is who they say they are."

**What We Have Achieved**

"We have established an SSL/TLS connection, ensuring that our traffic is secure and encrypted."

**SSL Cheat Sheet**

**`mkdir /etc/pki/CA`**

**`cd /etc/pki/CA`**

**`mkdir newcerts certs crl private requests`**

**`touch index.txt`**

**`echo '1234' > serial`**

**`openssl genrsa -out /etc/pki/CA/private/root.ca.key.pem 4096`**

**`openssl req -key /etc/pki/CA/private/root.ca.key.pem -new -x509 -days 7300 -extensions v3_ca -out root.ca.crt.pem`**

**`cp root.ca.crt.pem /etc/pki/ca-trust/source/anchors/`**

**`update-ca-trust extract`**

**`openssl genrsa -out net.key.pem 2048`**

**`openssl req -config /etc/pki/tls/openssl.cnf -key net.key.pem -new -out net.csr.pem`**

**`openssl ca -config /etc/pki/tls/openssl.cnf -extensions v3_req -days 3650 -in net.csr.pem -out net.crt.pem -cert root.ca.crt.pem`**

**`rm net.csr.pem`**

**`mv net.crt.pem /etc/pki/CA/certs`**

**`mv net.key.pem /etc/pki/CA/private`**

**`chmod -R 600 /etc/pki/CA`**

**Edit /etc/nginx/nginx.conf**

```bash
**server {
    listen 443 ssl;  # Listen on port 443 for SSL
    server_name example.net;  # Domain name for the server
    root /var/www/example.net/;  # Root directory for the website

    ssl_certificate /etc/pki/CA/certs/net.crt.pem;  # Path to the SSL certificate
    ssl_certificate_key /etc/pki/CA/private/net.key.pem;  # Path to the SSL certificate key

    access_log /var/log/nginx/example.net/access.log;  # Log file for access logs
    error_log /var/log/nginx/example.net/error.log;  # Log file for error logs
}**
```

**Edit /etc/ssl/openssl.cnf**

**`dir = /etc/pki/CA`**

**`certificate = $dir/root.ca.crt.pem`**

**`private_key = $dir/private/root.ca.key.pem`**