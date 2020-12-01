## XST or Cross-site tracing
 
XST is a means for accessing headers normally restricted from JavaScript. It was used to bypass httponly flag which is used to prevent scripts from accessing cookies.

To exploit XST, the target should be vulnerable to XSS and the http TRACE method should be allowed by the web server.

## Exploitation
 
If the http only cookie is set, then the script cannot access cookie and there is no benefit of presence of XSS.
Although, if TRACE method is allowed than the attacker can create a XMLHtttpRequest object within the javascript to trace the server's response header.
The response headers also contains the cookie which is now availale to the attacker.

## Downside to attacker
 
Normally http TRACE method is not used and is  just used by the developers for debugging purpose. Thus, modern browsers simply block TRACE method to prevent users from XST attack. For this reason the attack is not so useful.

REF : https://deadliestwebattacks.com/2010/05/18/cross-site-tracing-xst-the-misunderstood-vulnerability/