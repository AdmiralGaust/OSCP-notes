## What is distcc

distcc is designed to speed up compilation by taking advantage of unused processing power on other computers. A machine with distcc installed can send code to be compiled across the network to a computer which has the distccd daemon and a compatible compiler installed.
 
## Exploitation

The problem with this service is that an attacker can easily abuse it to run a command of their choice. consider the cve listed below

* __CVE-2004-2687__
 distcc 2.x, as used in XCode 1.5 and others, when not configured to restrict access to the server port, allows remote attackers to execute arbitrary commands via compilation jobs, which are executed by the server without authorization checks.

msf > use exploit/unix/misc/distcc_exec