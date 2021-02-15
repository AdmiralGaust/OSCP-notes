## Linux privilege Escalation

* Automated enumeration scripts

```
// http://pentestmonkey.net/tools/audit/unix-privesc-check
./unix-privesc-check > output.txt

// https://github.com/carlospolop/privilege-escalation-awesome-scripts-suite/tree/master/linPEAS
linpeas -a > /dev/shm/linpeas.txt
```

* Enumerating Users

```
id
whoami
cat /etc/passwd
```

* Get System information

```
cat /etc/issue
cat /etc/*-release
uname -a
```

* Check bash History

```
cat ~/.bash_history
```

* Enumerating Running Processes and Services

```
ps axu

https://github.com/DominicBreuker/pspy/
```

* Enumerating Networking Information

```
ip a
/sbin/route
netstat -anp
ss -anp
```

* Enumerating Firewall Status and Rules

```
Root privileges are required to list firewall rules with iptables
```

* Enumerating Scheduled Tasks

```
ls -lah /etc/cron*
cat /etc/crontab
grep "CRON" /var/log/cron.log
```

* Enumerating Installed Applications

```
dpkg -l
```

* Enumerating Readable/Writable Files and Directories

```
find / -writable -type d 2>/dev/null
find / -writable -type f 2>/dev/null
```

* Enumerating Unmounted Disks

```
/bin/lsblk
cat /etc/fstab
mount
```

* Enumerating Kernel Modules

```
// list modules
lsmod

// get details
/sbin/modinfo libata
```

* Enumerating Binaries That AutoElevate

```
find / -perm -u=s -type f 2>/dev/null
```

* Docker Escape/ Break out

```
https://book.hacktricks.xyz/linux-unix/privilege-escalation/docker-breakout
```