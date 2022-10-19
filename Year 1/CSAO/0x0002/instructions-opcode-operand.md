
# Instructions
- Building blocks of assembly programs
- In x86 assembly, an instruction is made of a mnemonic and either zero or more operands

|Mnemonic|Destination Operand|Source Operand|
|-|-|-|
|mov|rax|0x42|

# Operands
- Used to identify information used by instructions
- 3 Types
	- Immediate --> fixed values like 0x42
	- Registers --> refers to registers like rax
	- Memory address --> refers to a mem address that contains a value like [eax]


# Opcodes
- As the CPU cannot understand mov or add each of these correspond to an opcode to tell the CPU what operation to perform

|**Instruction**|**mov ecx,**|**0x42**|
|-|-|-|
|**Opcodes**|**B9**|**42 00 00 00**|


# System Calls
- 2 spaces in memory --> kernel and user space
	- User space --> responsible for running user applications (restricted)
	- Kernel space --> running kernel codes and system processes (privileged)
- Syscalls --> Used by user space applications to talk to the kernel space though libraies 