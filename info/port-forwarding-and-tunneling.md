## SSH Port Forwarding

__Local Forwarding__ - Binding to local machine port 8080 and forwarding to 10.10.10.203 on port 80 via vps (only localhost can connect)

```
ssh -N -L vps-ip:8080:10.10.10.203:80 vps_ip
```

__Remote forwarding__ - Remote ssh server listens on remote port 8080 and forwards all the traffic destined to remote port 8080 via ssh to destination host.(Opposite of Local Forwarding)

```
ssh -N -R vps-ip:8080:localhost:80 user@htb-ip
```

__Dynamic Forwarding__ - Dynamic port forwarding allows you to create a local SOCKS4 application proxy (-N -o) on our Kali Linux machine on TCP port 8080 (127.0.0.1:8080), which will tunnel all incoming traffic to any host in the target network, through the compromised Linux machine, which we log into as student (student@10.11.8.128):

```
kali@kali:~$ sudo ssh -N -D 127.0.0.1:8080 student@10.11.8.128
```

* Running below command on vps will make it act as a socks4 proxy on port 8080. We can then edit the /etc/proxychains.conf file and run tools via socks proxy.

```
ssh -N -D 96.126.72.56:8080 96.126.72.56
proxychains chromium
```

## Port forwarding on windows

* __Plink__ - plink also uses ssh for port forwarding and has similar syntax. However, to accept caching the key interactive prompt, we must pipe `echo y` output to plink.

```
// https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html
cmd.exe /c echo y | plink.exe -ssh -l root -pw toor -R 10.11.0.4:1234:127.0.0.1:3386 10.11.0.4
```

* __NETSH__

```
netsh interface portproxy add v4tov4 listenport=4455 listenaddress=10.11.8.22 connectport=445 connectaddress=192.168.1.118
```

## HTTP tunneling

For http tunneling, it needs to setup both http client and server. The client encapsulates the traffic into an HTTP stream and sends it to the server which then decapsulates it and forward to the destined port.

```
sudo apt install httptunnel

// http client
htc --forward-port 8888 10.11.0.128:1234

// http server 
hts --forward-port localhost:8888 1234
```

# Chisel

https://github.com/jpillora/chisel