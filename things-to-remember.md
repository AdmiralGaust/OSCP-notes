## Things to remember

* The first two fields in /etc/shadow denotes the username and password hash.

* For users with second field as * or !, the remote access is not allowed. 

* Ports below 1024 requires admin/root priveleges to create listener.

* Ensure to get & set execution policy if running powershell commands `Set-ExecutionPolicy Unrestricted` and `Get-ExecutionPolicy`

* When using public exploits, make sure the return address, shellcode and other key components are modified according to the needs.

* To change the admin user's password, we must switch to a high integrity command prompt. Otherwise, it will fail even if we are logged in as an administrative user.