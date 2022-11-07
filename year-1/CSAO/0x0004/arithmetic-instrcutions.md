# Status Flags
- Status flags are updated to indicate certain properties of the result
	- eg if the result is 0 then the Zero Flag is set
- Once a flag is set, it remains in that state until another instruction that affects the flag is executed
- Not all instructions affect all status flags 
	- mov, push, pop don't affect any flags
	- inc and dec affect all but the carry flag
	- add and sub affect all 6 flags


# Arithmetic Instructions

## inc and dec instructions
- The inc instruction increments the contents of its operand by one
- The dec instruction decrements the contents of its operand by one
```nasm
; Syntax

inc <REG>
inc <MEM>

dec <REG>
dec <MEM>
```

## add instruction
- add instruction adds together two operands, storing the result in the first operand
``` nasm
; Syntax
add <REG>,<REG>
add <REG>, <MEM>
add <MEM>,<REG>
add <REG>,<immediate value>
add <MEM>,<immediate value>
; Examples
add rax, 10 ; add 10 to the contents of RAX.
add BYTE PTR [var_1], 10 ; add 10 to the single byte stored at memory address “var_1”.
```


## adc instruction - add with carry
- adds together 2 operands and the value in the carry flag, stores the result in the first operand
- destination = destination + source + CF
``` nasm
; Syntax
adc destination,source
```

- Carry flag manipulating instructions
	- `stc` --> 'set carry flag', sets CF to 1
	- `clc` --> 'clear carry flag', clears CF to 0
	- `cmc` --> 'complement carry flag', inverts the value in the CF

## sub instruction
- subtracts the second operand from the first
``` nasm
; Syntax
sub <REG>,<REG>  
sub <REG>,<MEM>  
sub <MEM>,<REG>  
sub <REG>,<immediate value>  
sub <MEM>,<immediate value>  

; Examples  
sub rax, 10 ; subtract contents of RAX from the immediate value 10.  
sub [var_1], esi ; subtract the contents of ESI from the 32-bit ; integer stored at memory location “var”.
```

## sbb instruction
- subtracts the second operand from the first and then subtracts the value in the carry flag 
- Finally indicates the borrow in the CF
- destination = destination - source - CF
``` nasm
; Syntax
sbb destination,source
```