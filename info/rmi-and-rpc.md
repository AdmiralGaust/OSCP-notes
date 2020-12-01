## RPC
 
It is used by programs in distributed system to manage shared data.

Remote procedure call (RPC) is when a __computer program causes a procedure (subroutine) to execute in a different address space__ (commonly on another computer on a shared network), which is coded as if it were a normal (local) procedure call, without the programmer explicitly coding the details for the remote interaction. That is, the programmer writes essentially the same code whether the subroutine is local to the executing program, or remote
 
## RMI
 
 The RMI (Remote Method Invocation) is an API that provides a mechanism to create distributed application in java. The RMI allows an object to invoke methods on an object running in another JVM. The RMI provides remote communication between the applications using two objects stub and skeleton.
 
## Difference btw RPC and RMI
 
RPC is C based, and as such it has structured programming semantics, on the other side, RMI is a Java based technology and it's object oriented.

With RPC you can just call remote functions exported into a server, in RMI you can have references to remote objects and invoke their methods, and also pass and return more remote object references that can be distributed among many JVM instances, so it's much more powerful.

