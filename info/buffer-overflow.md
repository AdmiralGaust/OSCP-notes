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

## Roadmap to exploitation (stack based only)

* Try to crash the program with incrementing buffer length.

* 