# The Stack

## Review
- Memory used to store functions, local variables, and flow control
	- Last In First Out structure

- RSP/ESP (Stack pointer) - points to top of stack
- RBP/EBP (Base pointer) - points to bottom of stack

- Grows from high memory to low memory

- PUSH puts data onto the stack
	- 32 bit - decreases by 4 bytes at a time
	- 64 bit - decreases by 8 bytes at a time

- POP takes data off of the stack
	- 32 bit - increase by 4 bytes at a time
	- 64 bit - increase by 8 bytes at a time

