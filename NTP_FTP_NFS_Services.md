# NTP/FTP/NFS

**Why Configure Different Server Services?**

Various services can be configured on a server to meet the specific requirements of clients, applications, or developers. These services facilitate the deployment and management of applications hosted on the server. Additionally, different services are available to cater to the diverse needs of developers. Notably, NTP ensures accurate time and date synchronization, FTP enables secure file transfer and user authentication, and NFS serves as a storage solution for sharing files among multiple machines.

**How It Works**

**Network Time Protocol (NTP):**

Install the NTP software on your system.

Configure the /etc/ntp.conf file and specify the NTP servers you wish to sync with.

After restarting the NTP service, your system will synchronize with the configured NTP server. The below-mentioned commands can verify this.

**File Transfer Protocol (FTP):**

Configuring FTP involves the following steps:

Install the FTP server and open the necessary port.

Adjust the configuration settings to specify that FTP allows login only for users listed in the user-list file.

Define the root directory path for users with FTP access.

Add users to the Linux server and include their information in the user-list file.

Users can employ FTP clients like FileZilla or WinSCP to log in to the server for file transmission.

**Network File System (NFS):**

Setting up NFS requires the following steps:

Install both the NFS server and NFS client services and open the required ports.

On the server, edit the /etc/exports file to specify the directory to be shared, client IP addresses, and permissions for the NFS mount.

On the client side, create a mount point and add an NFS entry in the /etc/fstab file.

Mount the NFS on the client machine.

Verify the configuration using the exportfs -v and showmount -e server_ip commands on both the server and client.

Create a file on the server within the shared NFS directory and check whether it synchronizes with the client. If successful, you have established the NFS service.

**Cheat Sheet NTP**

**`yum install ntp`**

**`firewall-cmd --permanent --add-port=123/udp`**

**`vim /etc/ntp.conf`**

**`driftfile /var/lib/ntp/ntp.drift`**

**`server ntp_server_hostname_1 iburst`**

**`server ntp_server_hostname_2 iburst`**

**`server ntp_server_hostname_3 iburst`**

**`server ntp_server_hostname_4 iburst`**

**`server ntp_server_hostname_5 iburst`**

**`# By default, exchange time with everybody, but don't allow configuration.`**

**`restrict -4 default kod notrap nomodify nopeer noquery limited`**

**`restrict -6 default kod notrap nomodify nopeer noquery limited`**

**`# Local users may interrogate the ntp server more closely.`**

**`restrict 127.0.0.1`**

**`restrict ::1`**

**`systemctl restart ntp.service`**

**`ntpq -p`**

**`ntpdate -u your_server_ip`**

**`ntpstat`**

**Cheat Sheet FTP**

**`yum install vsftpd`**

**`firewall-cmd --permanent --add-port=21/tcp`**

**`vim /etc/vsftpd/vsftpd.conf`**

**`anonymous_enable=NO`**

**`local_enable=YES`**

**`write_enable=YES`**

**`chroot_local_user=YES`**

**`userlist_enable=YES`**

**`userlist_file=/etc/vsftpd/user_list`**

**`userlist_deny=NO`**

**`user_sub_token=$USER`**

**`local_root=/home/$USER`**

**`echo “testuser” >> /etc/vsftpd/user_list`**

**`adduser ftpuser`**

**`passwd ftpuser`**

**`mkdir –p /home/testuser`**

**`sudo /etc/init.d/vsftpd restart`**

**`sudo chkconfig vsftpd on`**

**Cheat Sheet NFS**

**`Server Side`**

**`dnf install nfs-utils`**

**`mkdir /var/nfs/general -p`**

**`chown nobody /var/nfs/general`**

**`ls -dl /var/nfs/general`**

**`vim /etc/exports`**

**`/var/nfs/general 13.233.129.179(rw,sync)`**

**`systemctl enable nfs-server`**

**`systemctl restart nfs-server`**

**`systemctl status nfs-server`**

**`firewall-cmd --permanent --add-service=nfs`**

**`firewall-cmd --permanent --add-service=mountd`**

**`firewall-cmd --permanent --add-service=rpc-bind`**

**`firewall-cmd --reload`**

**`firewall-cmd --permanent --list-all | grep services`**

`exportfs -v`

**Client Side**

**`dnf install nfs-utils`**

**`mkdir -p /nfs/general`**

**`vim /etc/fstab`**

**`13.234.48.211:/var/nfs/general /nfs/general nfs defaults 0 0`**

**`mount 13.234.48.211:/var/nfs/general /nfs/general`**

**`df -h`**

**`showmount -e 13.234.48.211`**

**`touch /n*fs/general/testfile*`**

**`*ls -dl /nfs/general/testfile*`**