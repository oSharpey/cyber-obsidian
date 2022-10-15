# Registers 
- Used to store data and instructions fetched from RAM during execution
- Different types of registers perform specific functions
- Different registers have different amount of data they can store (Widths)
	- **AL** - Lowest 8 bits
	- **AH** - Highest 8 bits 
	- **AX** - 16 Bits (high and low 8 bits together)
	- **E** - Extended 32 bits eg, EAX
	- **R** - Full 64 Bits (only in x64) eg, RAX

### IA 64 Registers
- General Registers
	- Used by CPU during execution
	- 16 registers, 64 bits each
- Segment Registers
	- Used to track sections of memory
	- 6 registers, 64 bits each
- Status Flag Register (RFLAG)
	- Used to make decisions
	- 1 register, 64 bit 
- Instruction Pointer
	- Stores address of next instruction to be executed 
	- 1 register, 64 bit


### Flags Register 
- Stores system states and logical results of executed code
- 32 bit register called **EFLAGS**
- This can be extended to x64 to a 64 bit register called **RFLAGS**
- Upper 32 bits of **RFLAGS** is reserved. The lower 32 bits act the same as **EFLAGS**

### Instruction Pointer
- Tracks the next instruction when a CPU loads a program (stores a pointer to the memory address of the next instruction to be executed )
- Controls the flow of a program
- Called **RIP** in x64 and **EIP** in x86
- **A good target if you want to manipulate execution order**
