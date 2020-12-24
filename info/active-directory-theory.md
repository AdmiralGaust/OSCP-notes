## Active Directory Theory

* When an instance of Active Directory is configured, a domain is created such as corp.com . Within this domain, we can add various types of objects, including computer and user objects. These objects are orgnized with the help of Organizational Units (OU).

* To gain complete control over all computers in AD, we could compromise a member of the Domain Admins group or a domain controller.

* Domain controller contains all the password hashes of every single domain user account.

* When applications like Exchange, SQL, or IIS are integrated into Active Directory, a unique service instance identifier known as a Service Principal Name (SPN) is used to associate a service on a specific server to a service account in Active Directory.

* By enumerating all registered SeitherPNs in the domain, we can obtain the IP address and port number of applications running on servers integrated with the target Active Directory.

## Active Directory Authentication

Active Directory uses either Kerberos or NTLM authentication protocols for most authentication attempts. By default, it uses kerberos for authentication.


* __NTLM Authentication__ - NTLM uses an encrypted challenge/response protocol to authenticate a user without sending the user's password over the wire.


1. Client initiates authentication request
2. Server responds with a challenge/nonce. The challenge is a 8-byte number which must be unpredictable.
3. Client and server both have a shared secret(NTLM hash of user password).
4. Client computes the response which is a function of secret and challenge. Mathematically, Response R=f(secret, challenge)
5. client sends the response to server.
6. server also calculates the response and matches it with the recieved response.
7. In this way, the client can be authenticated without actually transmitting the secret over unsecured channels.


* __Kerberos Authentication__ -  The Domain controller plays the key role in kerberos authentication. It stores password hashes of all the users in a lookup table and acts as the key distribution center KDC.


1. Client sends the Authentication Server Request (or AS_REQ) containing a time stamp encrypted with user's password hash and the username. 
2. DC looks up the password hash associated with
the specific user and attempts to decrypt the time stamp.
3. If the decryption process is successful
and the time stamp is not a duplicate (a potential replay attack), the authentication is considered
successful.
4. DC replies with an Authentication Server Reply (AS_REP) that contains a session key (since Kerberos is stateless) and a Ticket Granting Ticket (TGT).
5. The session key is encrypted using the user's password hash, and may be decrypted by the client and reused.
6. TGT contains information regarding the user(eg- group memberships) and the session key. To avoid tampering, it is encrypted by a secret key known only to the KDC.
7. KDC considers the client authentication complete now. By default, the TGT will be valid for 10 hours, after which a renewal occurs. This renewal does not require the user to re-enter the password.


__Accessing Resources using TGT__

8. Now, when the user wishes to access resources of the domain, the client constructs a TGS request containing username, TGT and SPN of the resource.
9. If the SPN exists in the domain, the TGT is decrypted using the secret key known only to the KDC. The session key is then extracted from the TGT and used to decrypt the username and timestamp of the request.
10. KDC replies with a session key(to be used btw client and application) and service ticket encrypted with password hash of SPN service account containing the username and group memberships of user.
11. Client sends request to application server containing the username, service ticket and a timestamp encrypted with the session key associated with the service ticket.
12. The application server decrypts the service ticket using the service account password hash.
13. Before access is granted, the service inspects the supplied group memberships in the service ticket and assigns appropriate permissions to the user.



## Cached Credential Storage and Retrieval

* Since Microsoft's implementation of Kerberos makes use of single sign-on, password hashes must be stored somewhere in order to renew a TGT request. In current versions of Windows, these hashes are stored in the Local Security Authority Subsystem Service (LSASS) memory space which is part of OS itself and runs as SYSTEM.

* To dump hashes we first need to have local admin privileges. We can then use mimikatz to dump the password hashes.

* We can also dump TGT and TGS tickets using mimikatz.

* TGS is encrypted using the SPN's password hash. If we are able to decrypt it using brute force or guessing (in a technique known as Kerberoasting), we will know the password hash of SPN. Further, we can crack this password hash to get clear text password of the service account.

* The user and group permissions in TGS are not verified by the application. If we can get SPN password, we can forge the TGS to become any user. This forged ticket is known as silver(or golden) ticket.

* If we get TGT of other user, we can impersonate to be that user.


## Lateral Movement

* __Pass The Hash Attacks__ 

The Pass the Hash (PtH) technique allows an attacker to authenticate to a remote system or service using a user's NTLM hash instead of the associated plaintext password. Note that this will not work for Kerberos authentication but only for server or service(like SMB) using NTLM authentication.

_Use it to extract NTLM hashes of all users who have previously logged in to the local machine and then utilize them to get shell as these users._

* __Overpass the Hash__

PtH techniques can be used to authenticate to only NTLM supported services. Thus, upgrading NTLM hash to TGT may be quite helpful.

Utilize Pth technique to get shell as different user. Once shell is obtained, perform any action which requires domain permissions. Doing this would subsequently create a TGT.
