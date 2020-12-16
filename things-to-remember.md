## Things to remember

* The first two fields in /etc/shadow denotes the username and password hash.

* For users with second field as * or !, the remote access is not allowed. 

* Ports below 1024 requires admin/root priveleges to create listener.

* Ensure to get & set execution policy if running powershell commands `Set-ExecutionPolicy Unrestricted` and `Get-ExecutionPolicy`