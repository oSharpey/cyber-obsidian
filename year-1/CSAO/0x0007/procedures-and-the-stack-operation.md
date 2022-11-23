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


# Procedures
- procedures in assembly can act as function fin high-level languages
- functions calls involve 2 main actions
	- linkage:
		- about getting to and returning from a function call correctly 
		- two instructions that handle the linkage, call and ret
			- call \<fun name>; calls a function, push the 64 bit RIP register and jump to \<fun name>
			- ret; returns from a function, pop the stack into the rip register, effecting a jump to the line after the call
	- Argument:
		- sends info
	- 

