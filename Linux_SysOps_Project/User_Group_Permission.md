# User, Group, Permission

**# Common Commands**

**`# useradd nilesh`**

**`# passwd nilesh`**

**`# groupadd nilgroup`**

**`# userdel username`**

**`# groupdel groupname`**

**`# usermod -aG groupname username`**

**`# chsh -s /bin/bash john`**

**`# id username`**

**`# id groupname`**

**`# PAM (Pluggable Authentication Module)`**

**`# pam_tally2 --user username --reset`**

**`# echo $SHELL`**

**# Environment Variable**

**`# vi ~/.bashrc For User`**

**`# JAVA_HOME=/usr/bin/java`**

**`# /etc/environment For Global`**

**`# source .bashrc`**

**`# source /etc/environment`**

**`# $PATH=/root/.local/bin:/root/bin:/sbin:/bin:/usr/sbin:/usr/bin`**

**`>> Create bin in home Dir, add script and run from anywhere`**

**# Important Files**

**`# cat /etc/group`**

**`# cat /etc/passwd`**

**`# cat /etc/shadow`**

**`# ll -la /etc/skel`**

**`# .bash_logout`**

**`# .bashrc`**

**`# .bash_profile`**

**`# /home/user/.bashrc`**

**`# /home/user/.bash_profile`**

**# Standard Permissions**

**`# chmod 600 filename`**

**`# chown username:groupname file_or_dir`**

**`# Special Permission`**

**`# chmod +s file_or_dir SUID and SGID`**

**`# chmod +t directory stickybit`**

**# ACL**

**`# getfacl filename_or_dirname`**

**`# setfacl -m u:nilesh:wrx filename`**

**`# setfacl -x u: username filename`**

**`# setfacl -b filename`**

**# SELINUX**

**`# sestatus`**

**`# getenforce`**

**`# setenforce 0 permissive`**

**`# setenforce 1 enforcing`**

**`# ls -z`**

**`# ps auxZ`**

**`# semanage permissive -a process_name`**

**`# semanage enforcing -a process_name`**

**`# chcon -R -t httpd_log_t /var/log/testhttpd`**

**`# restorecon -R /var/log/testhttpd`**

**`# semanage fcontext -l`**

**`# /etc/selinux/config`**

**`# SELINUX=disabled >> Disabled mode means SELinux is entirely inactive, providing no security features`**

**`# SELINUX= permissive >> SELinux logs denials but doesn't prevent the actions from occurring, whereas in Enforcing mode, denials lead to the prevention of those actions`**

**`# SELINUX= enforcing >> actively enforces security policies, preventing actions that violate the defined security context`**