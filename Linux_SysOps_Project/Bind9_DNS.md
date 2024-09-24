# DNS

# Title: Deploying DNS Server

### Description: Implement a DNS service that resolves domain names to IP addresses and vice versa.

### Installing bind9 DNS Server

```bash
yum install bind bind-utils
```

### Configuration of Primary DNS Server

**Client, Server, Domains Information**

```bash
linux.server.com.               192.168.128.129;        # Primary Name Server
linuxslave.server.com.          192.168.128.131;        # Secondary DNS Server
client                          192.168.128.130;        # Client
client                          192.168.128.1;          # Client
website1.linux.server.com       192.168.128.129         # Domain
website2.linux.server.com       192.168.128.129         # Domain
linux.server.com                192.168.128.129         # Domain

```

**Edit `/etc/named.conf`**

```bash
vi /etc/named.conf

listen-on port 53 { 127.0.0.1; 192.168.128.131; };
allow-query     { localhost; 192.168.128.130; 192.168.128.1; 192.168.128.131; };
include "/etc/named/named.conf.local";

allow-transfer { 192.168.128.131; };

```

**OR**

```bash
comment allow-query

allow-query { trusted; };

acl "trusted" {
        192.168.128.129;        # Primary Name Server
        192.168.128.131;        # Secondary DNS Server
        192.168.128.130;        # Client
        192.168.128.1;          # Client
        }
```

### Update permission and create the directory for forward and reverse lookup files

```bash
chmod 755 /etc/named
mkdir /etc/named/zones
```

**Edit `/etc/named/named.conf.local`**

```bash
vim /etc/named/named.conf.local

zone "linux.server.com" {
        type master;
        file "/etc/named/zones/db.linux.server.com";
   };

zone "168.192.in-addr.arpa" {
        type master;
        file "/etc/named/zones/db.192.168";
   };
```

### Edit forward lookup and reverse lookup zone files

**Edit `/etc/named/zones/db.linux.server.com`**

```bash
# Forward lookup file to convert Domain Name into IP Addresss

vim /etc/named/zones/db.linux.server.com

$TTL    300
@       IN      SOA     linux.server.com. admin.linux.server.com (
                              6         ; Serial
                            300         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL

; name servers - NS record
       IN      NS      linux.server.com.
       IN      NS      linuxslave.server.com.

; name servers - A record
linux.server.com.        IN      A       192.168.128.129
linuxslave.server.com.   IN      A       192.168.128.129

; host servers - A record
website1.linux.server.com.      IN      A       192.168.128.129
website2.linux.server.com.      IN      A       192.168.128.129
```

**Edit `/etc/named/zones/db.192.168`**
Make sure to update the serial number every time we update the zone files

```bash
vim /etc/named/zones/db.192.168

$TTL    300
@       IN      SOA     linux.server.com. admin.linux.server.com. (
                              6        ; Serial
                            300         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL

; name servers
      	 IN      NS      linux.server.com.
         IN      NS      linuxslave.server.com.

; PTR Records
129.128  IN      PTR     linux.server.com.
129.128  IN      PTR     website1.linux.server.com.
129.128  IN      PTR     website2.linux.server.com.
```

### Firewall Settings

```bash
firewall-cmd --permanent --add-port=53/udp
firewall-cmd --permanent --add-port=53/tcp
firewall-cmd --permanent --add-service=dns
firewall-cmd --reload; firewall-cmd --list-all
```

### Check Configurations

```bash
named-checkconf
named-checkzone linux.server.com /etc/named/zones/db.linux.server.com
named-checkzone 168.192.in-addr.arpa /etc/named/zones/db.192.168
systemctl restart named; systemctl enable named; systemctl status named
```

### Client Configurations

```bash
vi /etc/resolv.conf

search server.linux.com  # your private domain
nameserver 192.168.128.129  # Primary DNS Server
nameserver 192.168.128.131  # Secondary DNS Server
```

### Test DNS server

```bash
nslookup/dig linux.server.com
nslookup/dig website1.linux.server.com
nslookup/dig website2.linux.server.com
ping website1.linux.server.com
telnet 192.168.128.129 53
netstat -tulpn | grep 53
```

```bash
--------------------------------------------------------------------------------
129.128.168.192.in-addr.arpa    name = linux.server.com.
129.128.168.192.in-addr.arpa    name = website1.linux.server.com.
129.128.168.192.in-addr.arpa    name = website2.linux.server.com.
--------------------------------------------------------------------------------
PING website1.linux.server.com (192.168.128.129) 56(84) bytes of data.
64 bytes from linux.server.com (192.168.128.129): icmp_seq=1 ttl=63 time=0.857 ms
64 bytes from linux.server.com (192.168.128.129): icmp_seq=2 ttl=63 time=1.43 ms
--------------------------------------------------------------------------------
telnet 192.168.128.129 53
Trying 192.168.128.129...
Connected to 192.168.128.129.
--------------------------------------------------------------------------------
; <<>> DiG 9.18.12-0ubuntu0.22.04.2-Ubuntu <<>> linux.server.com.
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 19438
;; flags: qr aa rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 1, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 1232
; COOKIE: 2647c77b01cb6e349b1f6f1f64f22ff3c7489ab785236836 (good)
;; QUESTION SECTION:
;linux.server.com.              IN      A

;; ANSWER SECTION:
linux.server.com.       300     IN      A       192.168.128.129

;; AUTHORITY SECTION:
linux.server.com.       300     IN      NS      linux.server.com.

;; Query time: 439 msec
;; SERVER: 172.25.160.1#53(172.25.160.1) (UDP)
;; WHEN: Sat Sep 02 00:09:46 IST 2023
;; MSG SIZE  rcvd: 103
--------------------------------------------------------------------------------
Server:         172.25.160.1
Address:        172.25.160.1#53

Name:   linux.server.com
Address: 192.168.128.129
--------------------------------------------------------------------------------
netstat -tulpn | grep 53
tcp        0      0 192.168.128.129:53      0.0.0.0:*               LISTEN      4719/named
tcp        0      0 127.0.0.1:53            0.0.0.0:*               LISTEN      4719/named
tcp        0      0 127.0.0.1:953           0.0.0.0:*               LISTEN      4719/named
tcp6       0      0 :::53                   :::*                    LISTEN      4719/named
tcp6       0      0 ::1:953                 :::*                    LISTEN      4719/named
udp        0      0 192.168.128.129:53      0.0.0.0:*                           4719/named
udp        0      0 127.0.0.1:53            0.0.0.0:*                           4719/named
udp6       0      0 :::53                   :::*                                4719/named
```

### Configure Secondary DNS Server

**Edit `/etc/named.conf`**

```bash
vi /etc/named.conf

listen-on port 53 { 127.0.0.1; 192.168.128.131; };
allow-query     { localhost; 192.168.128.130; 192.168.128.1; 192.168.128.131; };
include "/etc/named/named.conf.local";

allow-transfer { 192.168.128.131; };
```

**OR**

```bash
comment allow-query

allow-query { trusted; };

acl "trusted" {
        192.168.128.129;        # Primary Name Server
        192.168.128.131;        # Secondary DNS Server
        192.168.128.130;        # Client
        192.168.128.1;          # Client
        }**
```

**Edit `/etc/named/named.conf.local`**

```bash

vim /etc/named/named.conf.local

zone "linux.server.com" {
    type slave;
    file "slaves/db.nyc3.example.com";
    masters { 192.168.128.129; };  # ns1 private IP
};

zone "168.192.in-addr.arpa" {
    type slave;
    file "slaves/db.192.168";
    masters { 192.168.128.129; };  # ns1 private IP
};
```

**Perform the configuration check and testing steps as mentioned previously**

**No need to create the zone files on the slave server manually. The slave server is configured to automatically request and retrieve the zone data from the master server specified in the masters directive.**

### Configuring logging on a BIND DNS server

- We can configure various logging option for different parameters of DNS servers.
- The /var/log/named/named.log file in BIND9 typically contains information about DNS server startup, errors, and warnings, DNS query activity, and potentially security events and zone transfers. The specific content may vary depending on system configuration and BIND9 version. It serves as a valuable resource for monitoring and troubleshooting DNS-related issues.

**Edit `/etc/named.conf`**

```bash
logging {

   channel default_log {
       file "/var/log/named/named.log" versions 3 size 5m;
       severity dynamic;
    };
       category default { default_log; };

};
```

**Create the log file Directory and update the permissions**

```bash
mkdir  /var/log/named/
chown named:named /var/named/log/
named-checkconf
systemctl restart named
tail -f /var/log/named/named.log
```
