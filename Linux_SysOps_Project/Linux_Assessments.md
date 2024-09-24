# Linux Assessments

# Assessment One

**Create the following users and group memberships:**

```bash
[root@L2-Linux-131 student]# groupadd linux
[root@L2-Linux-131 student]# useradd -G linux redhat
[root@L2-Linux-131 student]# useradd -G linux suse
[root@L2-Linux-131 student]# useradd -s /usr/bin/nologin debian
[root@L2-Linux-131 student]# passwd redhat
Changing password for user redhat.
New password:
BAD PASSWORD: The password fails the dictionary check - it is based on a dictionary
word
Retype new password:
passwd: all authentication tokens updated successfully.
[root@L2-Linux-131 student]# passwd suse
Changing password for user suse.
New password:
BAD PASSWORD: The password fails the dictionary check - it is based on a dictionary
word
Retype new password:
passwd: all authentication tokens updated successfully.
[root@L2-Linux-131 student]# passwd debian
Changing password for user debian.
New password:
BAD PASSWORD: The password fails the dictionary check - it is based on a dictionary
word
Retype new password:
passwd: all authentication tokens updated successfully.
[root@L2-Linux-131 student]# date
Wed Oct 18 14:15:27 GMT 2023
redhat:x:1001:1002::/home/redhat:/bin/bash
suse:x:1002:1003::/home/suse:/bin/bash
debian:x:1003:1004::/home/debian:/usr/bin/nologin
```

**Create a colloborative directory "/common/share".**

```bash
[root@L2-Linux-131 student]# mkdir -p /common/share
[root@L2-Linux-131 student]# chown :linux /common/share
[root@L2-Linux-131 student]# chmod 700 /common/share
[root@L2-Linux-131 student]# chmod 770 /common/share
[root@L2-Linux-131 student]# chmod o-rwx /common/share/
[root@L2-Linux-131 student]# chmod g+s /common/share
[root@L2-Linux-131 common]# pwd
/common
[root@L2-Linux-131 common]# ls -l
total 0
drwxrws---. 2 root linux 6 Oct 18 14:20 share
```

**Locate the files (only file, not directory) which are greater than 20MB in size and redirect the output to a file /root/find. Size.**

```bash
[root@L2-Linux-131 common]# find / -type f -size +20M -exec ls -lrth {} \; >
/root/find.size
[root@L2-Linux-131 common]# head /root/find.size
-rw-------. 1 root root 51M Jan 5 2023 /boot/initramfs-4.18.0-408.el8.x86_64.img
-rw-------. 1 root root 112M Jan 5 2023 /boot/initramfs-0-rescuea1051e78ee2842c98673b51399827d8e.img
-rw-------. 1 root root 27M Sep 26 03:10 /boot/initramfs-4.18.0-408.el8.x86_64kdump.img
-r--------. 1 root root 128T Oct 18 02:53 /proc/kcore
-rw-------. 1 root root 128M Oct 18 02:53
/sys/devices/pci0000:00/0000:00:0f.0/resource1
-rw-------. 1 root root 128M Oct 18 02:53
/sys/devices/pci0000:00/0000:00:0f.0/resource1_wc
-rw-r--r--. 1 root root 122M Jan 5 2023 /var/lib/rpm/Packages
-rw-r--r--. 1 root root 28M Aug 26 09:21 /var/cache/PackageKit/8/metadata/appstream-8-
x86_64/repodata/96163ce30b69db6eecf2207ce2baec232681c403354d692df2d3c6994ff0fdb9-
filelists.xml.gz
-rw-r--r--. 1 root root 109M Jan 5 2023 /var/cache/PackageKit/8/metadata/appstream-8-
x86_64/packages/firefox-102.4.0-1.el8.x86_64.rpm
-rw-r--r--. 1 root root 56M Jan 5 2023 /var/cache/PackageKit/8/metadata/appstream-8-
x86_64/packages/llvm-compat-libs-14.0.6-1.module_el8.8.0+1224+64629835.x86_64.rpm
```

**Create Native Linux Partition:**

```bash
[root@L2-Linux-131 common]# df -PTh | grep vfatdata
/dev/sdb2 vfat 2.0G 616K 2.0G 1% /vfatdata
[root@L2-Linux-131 common]# ll /vfatdata/
total 612
-rwxr-xr-x. 1 root root 626050 Oct 18 15:02 messages
[root@L2-Linux-131 common]#
[root@L2-Linux-131 common]# cat /etc/fstab | grep vfatdata
/dev/sdb2 /vfatdata vfat defaults 0 0
```

**Create LV:**

```bash
[root@L2-Linux-131 student]# pvs
PV VG Fmt Attr PSize PFree
/dev/sda3 cs lvm2 a-- 78.41g 0
/dev/sdb1 wcvg lvm2 a-- <5.00g 4.68g
[root@L2-Linux-131 student]# vgs
VG #PV #LV #SN Attr VSize VFree
cs 1 3 0 wz--n- 78.41g 0
wcvg 1 1 0 wz--n- <5.00g 4.68g
[root@L2-Linux-131 student]# lvs
LV VG Attr LSize Pool Origin Data% Meta% Move Log Cpy%Sync Convert
home cs -wi-ao---- 23.13g
root cs -wi-ao---- 47.38g
swap cs -wi-ao---- 7.89g
wclv wcvg -wi-ao---- 320.00m
[root@L2-Linux-131 student]# ll /worldcup2023/
total 123954
drwx------. 2 root root 12288 Oct 18 17:03 lost+found
-rw-r--r--. 1 root root 126914560 Oct 18 17:05 Packages
```

**Install "mariadb" package.**

```bash
[root@L2-Linux-131 student]# rpm -qa | grep mariadb
mariadb-common-10.3.28-1.module_el8.3.0+757+d382997d.x86_64
mariadb-10.3.28-1.module_el8.3.0+757+d382997d.x86_64
mariadb-connector-c-config-3.1.11-2.el8_3.noarch
mariadb-connector-c-3.1.11-2.el8_3.x86_64
[root@L2-Linux-131 student]# echo "Total number of files: $(rpm -ql mariadb | wc -l)" >
/var/db-details
[root@L2-Linux-131 student]# cat /var/db-details
Total number of files: 48
[root@L2-Linux-131 student]# echo "Total number of docs: $(rpm -qd mariadb | wc -l)" >>
/var/db-details
[root@L2-Linux-131 student]# cat /var/db-details
Total number of files: 48
Total number of docs: 13
[root@L2-Linux-131 student]# echo "Total number of configs: $(rpm -qc mariadb | wc -l)"/var/db-details
[root@L2-Linux-131 student]# cat /var/db-details
Total number of files: 48
Total number of docs: 13
Total number of configs: 1
```

**Find total number of folders (only folders) in /var folder and save it in A file /root/var_folders. The contents should be having the total number of folders in /var folder is xxx.**

```bash
[root@L2-Linux-131 student]# echo "The total number of folders in /var folder is $(find
/var -type d | wc -l)" > /root/var_folders
[root@L2-Linux-131 student]# cat /root/var_folders
The total number of folders in /var folder is 904
```

**Create a script named "/root/disp-msg.sh" so that it should print the Below message 10 times while executing.**

```bash
[root@L2-Linux-131 student]# ll /root/disp-msg.sh
-rwxr-xr-x. 1 root root 101 Oct 18 17:39 /root/disp-msg.sh
[root@L2-Linux-131 student]# cat /root/disp-msg.sh
#!bin/bash
for ((i = 1; i <=10; i++)); do
echo "Happy 2023 WorldCup!!!" > /dev/pts/0
sleep 2
done
[root@L2-Linux-131 ~]# cp /root/disp-msg.sh /usr/local/bin/
[root@L2-Linux-131 ~]# which [disp-msg.sh](http://disp-msg.sh/)
/usr/local/bin/disp-msg.sh
[root@L2-Linux-131 ~]# crontab -l
*/20 * * * * /usr/local/bin/disp-msg.sh
Output for /dev/pts/1
[root@L2-Linux-131 ~]# ./disp-msg.sh
Happy 2023 WorldCup!!!
Happy 2023 WorldCup!!!
Happy 2023 WorldCup!!!
Happy 2023 WorldCup!!!
Happy 2023 WorldCup!!!
Happy 2023 WorldCup!!!
Happy 2023 WorldCup!!!
Happy 2023 WorldCup!!!
Happy 2023 WorldCup!!!
Happy 2023 WorldCup!!!
[root@L2-Linux-131 ~]#
```

**Create a user with the below requirements:**

```bash
[root@L2-Linux-131 ~]# useradd -u 2023 -c "Cricket Players" -m -d /mnt/wcindia -s
/bin/bash wcindia
[root@L2-Linux-131 ~]# cat /etc/passwd | grep wcindia
wcindia:x:2023:2023:Cricket Players:/mnt/wcindia:/bin/bash
[root@L2-Linux-131 ~]# usermod -a -G linux,batnball wcindia
[root@L2-Linux-131 ~]# groups wcindia
wcindia : wcindia linux batnball
```

**Create a Linux native filesystem:**

```bash
[root@L2-Linux-131 ~]# fdisk -l /dev/sdb
Disk /dev/sdb: 20 GiB, 21474836480 bytes, 41943040 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: dos
Disk identifier: 0x16507d63
Device Boot Start End Sectors Size Id Type
/dev/sdb1 2048 10487807 10485760 5G 8e Linux LVM
/dev/sdb2 10487808 31459327 20971520 10G 83 Linux
[root@L2-Linux-131 ~]# df -PTh | grep nativefs
/dev/sdb2 ext4 9.8G 24K 9.3G 1% /nativefs
[root@L2-Linux-131 ~]# cat /etc/fstab | grep nativefs
/dev/sdb2 /nativefs ext4 defaults 0 0
[root@L2-Linux-131 ~]# ll /root/nativefs_fs_alerting.sh
-rwxr-xr-x. 1 root root 204 Oct 18 18:52 /root/nativefs_fs_alerting.sh
[root@L2-Linux-131 ~]# cat /root/nativefs_fs_alerting.sh
#!/usr/bin/bash
threshold=70
usage=$(df -h /nativefs | awk 'NR==2 {print $5}' | tr -d '%' )
if [ "$usage" -ge "$threshold" ]; then
echo "/nativefs crossing 70% - Take some action" >> /opt/alert-msg
fi
[root@L2-Linux-131 ~]# crontab -l
*/20 * * * * /usr/local/bin/disp-msg.sh
*/2 * * * * /root/nativefs_fs_alerting.sh
[root@L2-Linux-131 ~]# cat /opt/alert-msg
/nativefs crossing 70% - Take some action
/nativefs crossing 70% - Take some action
[root@L2-Linux-131 student]# uptime
19:05:47 up 1 min, 2 users, load average: 1.14, 0.52, 0.19
[root@L2-Linux-131 student]# date
Wed Oct 18 19:05:49 GMT 2023
```

# Assessment Two

**Find the words which starts with "mang" by using linux dictionary. Store the words as content on the file /root/dictwords. 
(hint: folder named "dict" is available on your vm)**

```bash
[root@L2-Linux-131 ~]# grep '^mang' /usr/share/dict/words > /root/dictwords
[root@L2-Linux-131 ~]# head /root/dictwords
mang
manga
mangabeira
mangabev
mangabey
mangabeys
mangabies
mangaby
mangal
mangan
```

**Create users and groups as per the below requirements:**

```bash
[root@L2-Linux-131 ~]# groupadd -g 3010 sfgroup
[root@L2-Linux-131 ~]# useradd -m -G sfgroup sfprod1
[root@L2-Linux-131 ~]# useradd -m -G sfgroup sfprod2
[root@L2-Linux-131 ~]# useradd -m -G sfgroup sfprod3
[root@L2-Linux-131 ~]# useradd -M -N -s /sbin/nologin sfprod4
[root@L2-Linux-131 ~]# useradd -N sfvendor
[root@L2-Linux-131 ~]# cat /etc/passwd | grep sf
sfprod1:x:2024:2024::/home/sfprod1:/bin/bash
sfprod2:x:2025:2025::/home/sfprod2:/bin/bash
sfprod3:x:2026:2026::/home/sfprod3:/bin/bash
sfprod4:x:2027:100::/home/sfprod4:/sbin/nologin
sfvendor:x:2028:100::/home/sfvendor:/bin/bash
```

**Create a project folder which should meet out the below requirements:**

```bash
[root@L2-Linux-131 ~]# mkdir /sfprojest
[root@L2-Linux-131 ~]# chown sfprod1:sfgroup /sfprojest
[root@L2-Linux-131 ~]# chmod 770 /sfprojest
[root@L2-Linux-131 ~]# chmod g+s /sfprojest
[root@L2-Linux-131 /]# ll | grep sfprojest
drwxrws---. 2 sfprod1 sfgroup 6 Oct 18 19:31 sfprojest
```

**The user "sfvendor" should be able to navigate inside /sfproject folder and able to create files and folders.**

```bash
[root@L2-Linux-131 /]# setfacl -m u:sfvendor:rwx /sfprojest
[root@L2-Linux-131 /]# getfacl /sfprojest/
getfacl: Removing leading '/' from absolute path names
file: sfprojest/
owner: sfprod1
group: sfgroup
flags: -suser::rwx
user:sfvendor:rwx
group::rwx
mask::rwx
other::---
```

**Note: Disk of 20G (with standard HDD) should be attached to your VM.**

```bash
[root@L2-Linux-131 /]# df -PTh | grep sfnative
/dev/sdb3 ext3 2.0G 368K 1.9G 1% /sfnative
[root@L2-Linux-131 /]# ll /sfnative/
total 292
drwx------. 2 root root 16384 Oct 18 19:43 lost+found
-rw-------. 1 root root 275778 Oct 18 19:44 messages
```

**Create a lv named "sflv1" with a size of 40PE where the size of each PE should be 16MiB**

```bash
root@L2-Linux-131 /]# lvs
LV VG Attr LSize Pool Origin Data% Meta% Move Log Cpy%Sync Convert
home cs -wi-ao---- 23.13g
root cs -wi-ao---- 47.38g
swap cs -wi-ao---- 7.89g
sflv1 wcvg -wi-a----- 640.00m
wclv wcvg -wi-a----- 320.00m
[root@L2-Linux-131 /]# df -PTh | grep sflvdata
/dev/mapper/wcvg-sflv1 xfs 635M 158M 477M 25% /sflvdata
[root@L2-Linux-131 /]# ll /sflvdata/
total 124064
-rw-r--r--. 1 root root 127041536 Oct 18 19:52 Packages
```

**The FS /sfexistlv needs to be reduced to 1500M without any data loss.**

```bash
[root@L2-Linux-131 /]# lvs
LV VG Attr LSize Pool Origin Data% Meta% Move Log Cpy%Sync Convert
home cs -wi-ao---- 23.13g
root cs -wi-ao---- 47.38g
swap cs -wi-ao---- 7.89g
sfexistlv wcvg -wi-a----- 2.00g
sflv1 wcvg -wi-ao---- 640.00m
wclv wcvg -wi-a----- 320.00m
root@L2-Linux-131 /]# df -PTh | grep sfexistlv
/dev/mapper/wcvg-sfexistlv ext4 2.0G 24K 1.8G 1% /sfexistlv
[root@L2-Linux-131 /]# umount /sfexistlv
[root@L2-Linux-131 /]# e2fsck -f /dev/wcvg/sfexistlv
e2fsck 1.45.6 (20-Mar-2020)
Pass 1: Checking inodes, blocks, and sizes
Pass 2: Checking directory structure
Pass 3: Checking directory connectivity
Pass 4: Checking reference counts
Pass 5: Checking group summary information
/dev/wcvg/sfexistlv: 11/131072 files (0.0% non-contiguous), 26156/524288 blocks
[root@L2-Linux-131 /]# resize2fs /dev/wcvg/sfexistlv 1500M
resize2fs 1.45.6 (20-Mar-2020)
Resizing the filesystem on /dev/wcvg/sfexistlv to 384000 (4k) blocks.
The filesystem on /dev/wcvg/sfexistlv is now 384000 (4k) blocks long.
[root@L2-Linux-131 /]# mount /dev/wcvg/sfexistlv /sfexistlv
[root@L2-Linux-131 /]# df -kh | grep sfexistlv
/dev/mapper/wcvg-sfexistlv 1.4G 24K 1.3G 1% /sfexistlv
```

**Find total number of files (only files) in /etc. folder and save it in a file /root/etc_files. The contents should be having the total number of files in /etc folder is xxx**

```bash
[root@L2-Linux-131 /]# echo "The total number of files in /etc folder is $(find /etc -
type f | wc -l)" > /root/etc_files
[root@L2-Linux-131 /]# cat /root/etc_files
The total number of files in /etc folder is 1307
9. Create a Monitoring script such that, it has to monitor the /sfnative FS:
[root@L2-Linux-131 /]# ll /root/monitoring_script.sh
-rwxr-xr-x. 1 root root 608 Oct 18 20:49 /root/monitoring_script.sh
```

```bash
[root@L2-Linux-131 /]# cat /root/monitoring_script.sh
#!/usr/bin/bash
DISK_THRESHOLD=70
EMAIL_RECIPIENT="root@localhost"
while true; do
INODE_USAGE=$(df -i /sfnative | awk 'NR==2 {print $5}' | sed 's/%//')
echo "Inode usage in /sfnative FS is $INODE_USAGE%"
DISK_USAGE=$(df -h /sfnative | awk 'NR==2 {print $5}' | sed 's/%//')
if [ "$DISK_USAGE" -ge "$DISK_THRESHOLD" ]; then
dd if=/dev/zero of=/sfnative/simulated_file bs=1M count=1000 # We can use this command from terminal to simulation and testing purpose
echo "High Disk Space Usage: /sfnative is at $DISK_USAGE%" | mailx -s "High
Disk Space Utilization" "EMAIL_RECIPIENT"
fi
sleep 300
done
```

```bash
[root@L2-Linux-131 /]# crontab -l
*/20 * * * * /usr/local/bin/disp-msg.sh
*/2 * * * * /root/nativefs_fs_alerting.sh
*/5 * * * * /root/monitoring_script.sh
```

L**ogin as the user sfprod1, trigger CPU stress (hint: using stress command) Using script top.sh**

```bash
[root@L2-Linux-131 /]# ll /root/top.sh
-rwxr-xr-x. 1 root root 514 Oct 18 21:17 /root/top.sh
```

```bash
#!/usr/bin/bash
user_to_monitor="sfprod1"
threshold=50
while true; do
cpu_utilization=$(top -b -n 1 -u "$user_to_monitor" | awk 'NR>7 {sum += $9} END
{print sum}')
if [ "$(awk -v cpu_utilization="$cpu_utilization" -v threshold="$threshold"
'BEGIN {print (cpu_utilization >= threshnold)}')" -eq 1 ]; then
subject="CPU UTILIZATION IS CROSSING 50% -FYI"
message="CPU utilization for user $user_to_monitor is
$cpu_utilization%."
echo "$message" | mailx -s "$subject" root@localhost
fi
sleep 60
done
```
