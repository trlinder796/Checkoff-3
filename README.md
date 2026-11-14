# Checkoff-3
Repository containing my research and example code for Checkoff 3

Stack Buffer Overflow Notes

A buffer overflow happens when a program writes more data into a buffer than it was allocated to hold, so the extra data spills into adjacent memory.
A stack buffer overflow is when that buffer lives on the stack (like a local array in a function).
The stack grows toward lower memory addresses, but arrays are written toward higher addresses. That’s why overflowing a local array can reach other data above it.
A function’s stack frame holds local variables, saved base pointer (EBP/RBP), and the return address.
Overflowing a local buffer can overwrite the saved base pointer and the return address.
The return address tells the CPU where to resume after the function finishes. If an attacker controls it, they control what code runs next.
Common vulnerable C functions: gets(), strcpy(), strcat(), sprintf(), scanf("%s"). None of them check the destination buffer size.
Safer alternatives: fgets(), strncpy(), strncat(), snprintf(). They still need correct size arguments.
C doesn’t do automatic bounds checking, which is why this is mostly a C/C++ problem.
Possible results of an overflow: a crash (segmentation fault), corrupted data, or arbitrary code execution.
Overwriting nearby local variables can change program behavior without touching the return address.
Classic attack idea: put malicious code in the buffer, then overwrite the return address to point back to it.
Defenses:
Stack canaries: a random value placed before the return address and checked before the function returns. If it changed, the program aborts.
ASLR (Address Space Layout Randomization): randomizes memory addresses so attackers can’t predict where things are.
NX/DEP (non-executable stack): marks the stack as non-executable so injected code can’t run.
Bounds checking and safe coding: use size-limited functions, validate input length.
Attackers got around NX with return-oriented programming (ROP), which chains together small snippets of existing code (gadgets) instead of injecting new code.
Compiler flags: -fstack-protector enables canaries; -fno-stack-protector disables them (often used in class demos).
Tools for finding these bugs: Valgrind, AddressSanitizer (-fsanitize=address), fuzzers, static analyzers.
Famous example: the Morris Worm (1988) exploited a buffer overflow in the fingerd service.
Root cause is always the same: trusting input length without checking it.


Reflection:

