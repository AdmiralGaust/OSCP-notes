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