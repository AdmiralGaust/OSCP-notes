## Coyote

Coyote is a Connector component for Tomcat that supports the HTTP 1.1 protocol as a web server. This allows Catalina, nominally a Java Servlet or JSP container, to also act as a plain web server that serves local files as HTTP documents.

## AJP

By default, Apache Tomcat listens on 3 ports, 8005, 8009 and 8080.

Port 8005 is less interesting and only allows shutting down the Tomcat server, while port 8009 hosts the exact same functionality as port 8080. The only difference being that __port 8009 communicates with the Apache JServ Protocol(AJP) while port 8080 uses HTTP__.

The Apache JServ Protocol (AJP) is essentially an optimized __binary version of HTTP.__ This makes communication with the AJP port rather difficult using conventional tools. The simplest solution is to __configure Apache as a local proxy, and use common tools such as Hydra and Metasploit to exploit the Tomcat server over AJP by launching attack against local apache webserver instance.__

Useful link
http://web.archive.org/web/20200311005332/https://ionize.com.au/exploiting-apache-tomcat-port-8009-using-apache-jserv-protocol/