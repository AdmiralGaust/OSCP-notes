## Things to remember

* The first two fields in /etc/shadow denotes the username and password hash. For users with second field as * or !, the remote access is not allowed. 

* /etc/passwd is prefered over /etc/shadow for authentication if password hash is available in passwd file.

* Ports below 1024 requires admin/root priveleges to create listener.

* Ensure to get & set execution policy if running powershell commands `Set-ExecutionPolicy Unrestricted` and `Get-ExecutionPolicy`

* When using public exploits, make sure the return address, shellcode and other key components are modified according to the needs.

* To change the admin user's password, we must switch to a high integrity command prompt. Otherwise, it will fail even if we are logged in as an administrative user.

* NTLM/LM password hashes are not salted(no timestamp either) and remain static between sessions making pass-the-hash attacks possible.

* TGT can only be used on the machine it was created for, but the TGS potentially offers more flexibility.

* The user and group permissions in TGS are not verified by the application. It blindly trusts it.