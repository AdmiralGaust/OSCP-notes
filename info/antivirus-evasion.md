## Antivirus Detection Mechanisms

* __Signature-Based Detection__ : An antivirus signature is a continuous sequence of bytes within malware that uniquely identifies it. In this method, the filesystem is scanned for known malware signatures and if any are detected, the offending files are quarantined.

* __Heuristic-Based Detection__ : In this method, the antivirus software looks for various patterns and program calls inside the binary instruction set to determine whether or not an action is considered malicious. This is achived by decompiling the binary and statically analyzing the source code.

* __Behavior-Based Detection__ : In this method, Antivirus software dynamically analyzes the behavior of a binary file by executing the file in question in an emulated environment, such as a small virtual machine, and looking for behaviors or actions that are considered malicious.


__Majority of antivirus developers use a combination of these detection methods to achieve higher detection rates__.


## Bypassing Antivirus Detection

1. __On-Disk Evasion__ - Focuses on obfuscating or altering the source code of file to bypass detection.

	
	* __Packers__ - Packers generate an executable that is not only smaller, but is also functionally equivalent with a completely new binary structure.The resultant file has a new signature and as a result, can effectively bypass older and more simplistic AV scanners.

	* __Obfuscators__ - Obfuscators reorganize and mutate code in a way that makes it more difficult to reverse-engineer. This includes replacing instructions with semantically equivalent ones, inserting irrelevant instructions or "dead code", 412 splitting or reordering functions, and so on.

	* __Crypters__ - ·crypter" software cryptographically alters executable code, adding a decrypting stub that restores the original code upon execution. This decryption happens in-memory, leaving only the encrypted code on-disk. Encryption has become foundational in modern malware as one of the most effective AV evasion techniques.

	* __Software Protectors__ - It uses a combination of all of the previous techniques in addition to other advanced ones, including anti-reversing, anti-debugging, virtual machine emulation detection, and so on. The Enigma Protector is a commercial tool available.

2. __In-Memory Evasion__ - Rather than obfuscating a malicious binary, creating new sections, or changing existing permissions, this technique instead focuses on the manipulation of volatile memory. One of the main benefits of this technique is that it does not write any files to disk, which is one the main areas of focus for most antivirus products.


	* __Remote Process Memory Injection__ - This technique attempts to inject the payload into another valid PE that is not malicious. The most common method of doing this is by leveraging a set of Windows APIs. First. we would open the process, allocate memory in the context of that process and copy the malicious payload to the newly allocated memory. Then it is executed in a seperate thread.

	* __Reflective DLL Injection__ - Unlike regular DLL injection, which implies loading a malicious DLL from disk using the LoadLibrary API, this technique attempts to load a DLL stored by the attacker in the process memory.The Windows operating system does not expose any APIs for loading a DLL from memory which is why attacker must write their own version of API.

	* __Process Hollowing__ - Attackers first create a non-malicious process in a suspended state. Once launched, the image of the process is removed from memory and replaced with a malicious executable image. Finally, the process is then resumed and malicious code is executed instead of the legitimate process.

	* __Inline hooking__ - As the name suggests, this technique involves modifying memory and introducing a hook(instructions that redirect the code execution) into a function to point the execution flow to our malicious code. Upon executing our malicious code, the flow will return back to t he modified function and resume execution, appearing as if only the original code had executed.


## Shellter - Advanced Tool for AV evasion

It uses a number of novel and advanced techniques to essentially backdoor a valid and non-malicious executable file with a malicious shellcode payload. 

It essentially performs a thorough analysis of the target PE file and then determines where it can inject our shellcode. Finally, Shellter attempts to use the existing PE Import Address Table (IAT) entries to locate functions that will be used for the memory allocation, transfer, and execution of our payload.

```
// shellter is designed to run on Windows
sudo apt install shellter
sudo apt install wine
```