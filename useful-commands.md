# Nmap Scan

`nmap -sC -sV -oA nmap-out example.com`

# SCP to copy files

Upload file/folder
`scp -r local.txt host:/path/to/dest`

Download file/folder
`scp -r host:/path/to/remote.txt .`

# Directory Brute Force

Dirsearch for brute forcing file system
`./dirsearch.py -r -e old,new,txt,asp,aspx,jsp,jspx,bak,gz,zip,tar,7z,php,htm,html,bat,sh,do -b --random-agents --plain-text-report=dirsearch.out -u url -w wordlist(optional)`

FFuf to fuzz content
`ffuf -w content_discovery_all.txt -u http://host/ -o dirfuzz.txt -of md -timeout 30 -ac -s -t 20 -se`

# SSH Port Forwarding

__Local Forwarding__ - Binding to local machine port 8080 and forwarding to 10.10.10.203 on port 80 via vps (only localhost can connect)
`ssh -L 8080:10.10.10.203:80 vps_ip`

__Remote forwarding__ - Remote ssh server listens on remote port 8080 and forwards all the traffic to remote port 8080 via ssh client to destination host.
`ssh -R 8080:localhost:80 public.example.com`

__Dynamic Forwarding__ - Dynamic port forwarding allows you to create a socket on the local (ssh client) machine, which acts as a SOCKS proxy server. Running below command on vps will make it act as a socks proxy on port 8080.
`ssh -D vps_ip:8080 vps_ip`