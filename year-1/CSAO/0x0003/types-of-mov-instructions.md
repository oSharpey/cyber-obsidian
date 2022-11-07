# MOV instruction recap
**Usage**
- **✓** mov register, immediate value  
- **✓** mov register, memory  
- **✓** mov memory, register  
- **✗** mov memory, memory - **illegal**

# LEA Instruction
- Load Effective Address
	- `lea ebx, [0x403000`  --> loads the address 0x403000 into ebx
	- LEA does not follow the default convention of using square brackets means fetch the value at that address

# XCHG Instruction
- Exchanges the contents of the destination (first operand) and source (second operand )
	- `XCHG Regsiter, Register`
	- `XCHG Register, Memory`
