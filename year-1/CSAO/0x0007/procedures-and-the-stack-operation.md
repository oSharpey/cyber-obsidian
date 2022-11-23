# The Stack Review
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
	- Argument Transmission:
		- sends information to the function and obtain a result
			- transmitting values to a function: call-by-value
			- transmitting address to a function: call-by-reference


# Parameter Passing
- A combination of registers and the stack is used to pass parameters to and/or from a function
- The first 6 integer arguments are passed in registers as follows
|Argument #|64-bit|32-bit|
|-|-|-|
|1|RDI|EDI|
|2|RSI|ESI|
|3|RDX|EDX|
|4|RCX|ECX|
|5|R8|R8D|
|6|R9|R9D|


# Calling convention - Register use
- Some registers are are expected to be preserved across a function call

# Stack frame layout
- From the 7th argument onward the arguments will be stored on the stack as shown below

