## Improper file permissions on service executable

1. Check Running services
2. Verify improper file permissions on service executable file (use icacls to check)
3. Replace the service executable binary with malicious exe
4. Try to restart the service
5. May be reboot the windows if cannot restart the service. Verify permissions first

```
// check if service auto restart on reboot
wmic service where caption="Serviio" get name, caption, state, startmode

// check if we have privilege to shut down system
whoami /priv

// reboot
shutdown /r /t 0
```


## Leveraging Unquoted Service Paths

1. Enumerate Services
2. Check if any of the high privilege service has spaces in path and the path is unquoted
3. Check if we have permission to write to any of the folder in the path 
4. Let say we have a service stored in a path such as `C:\Program Files\My Program\My Service\service.exe`. If the service path is stored unquoted, whenever Windows starts the service, it will attempt to run an executable from the following paths:
```
C:\Program.exe
C:\Program Files\My.exe
C:\Program Files\My Program\My.exe
C:\Program Files\My Program\My service\service.exe
```
5. If we can write, place the malicious binary to shortest path and restart the service(or reboot machine).



## Windows Kernel Vulnerabilities

1. Enumerate drivers (`driverquery /v`)
2. Look for third party drivers installed such as USBPcap
3. Search for known kernel/driver exploits
4. Cross-verify the driver version. Should be exact match.