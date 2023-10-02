# Bitwise Operations
- Assembly instructions that operate on the bits

## and instructions - Bitwise AND (&& or &)
- Performs a logical AND operation on 2 operands and places the result in the first operand 
- Both operands cannot be in memory
- The destination operand cannot be an immediate 
``` nasm
; Syntax 
and <dest>, <src>  

; Examples 
and ax, bx  
and rcx, rdx  
and eax, dword [dNum]  
and qword [qNum], rdx
```

## or instruction - Bitwise OR (| or ||)
- Performs a logical OR operation on 2 operands and places the result in the first operand 
- Both operands cannot be in memory
- The destination operand cannot be an immediate 
```nasm
; Syntax
or <dest>, <src>  

; Examples
or ax, bx  
or rcx, rdx  
or eax, dword [dNum]  
or qword [qNum], rdx
```

## xor instruction - Bitwise XOR (^)
- Performs a logical XOR operation on 2 operands and places the result in the first operand 
- Both operands cannot be in memory
- The destination operand cannot be an immediate 
```nasm
; Syntax
xor <dest>, <src>  

; Examples
xor ax, bx  
xor rcx, rdx  
xor eax, dword [dNum]  
xor qword [qNum], rdx
```

## not instruction - Bitwise NOT (~ or !)
- Performs a logical not operation
- operand cannot be an immediate
```nasm
; Syntax 
not <op>  

; Examples 
not ax  
not rdx  
not dword [dNum]  
not qword [qNum]
```

# Bit Shifting Operations
- `shl` (logical left shift)
- `sal` (arithmetic shift left)
- `shr` (logical shift right)
- `sar` (arithmetic shift right)

## shl and sal instructions
- both perform the same operation 
- shift the source operand left by from 1 to 31 bit positions 
- Empty bit positions are cleared
- the CF flag is loaded with the MSB that got shifted out of the operand 
- the immediate number is the number if places to shift
- the destination operand cannot be immediate 
```nasm
; Syntax
shl <dest>, <imm>  
sal <dest>, <imm>  
sal <dest>, cl  
shl <dest>, cl  

; Examples 
shl ax, 8  
sal rcx, 32  
shl rax, cl  
sal qword [qNum], cl
```

![[shl-sal-diagram.png]]


## shr instruction 
- Shift the source operand right by from 1 to 31 bits
- empty bit positions are cleared
- the CF flag is loaded with the LSB that got shifted out of the operand
- the immediate number is the number if places to shift
- the destination operand cannot be immediate 
```nasm
; Syntax 
shr <dest>, <imm>  
shr <dest>, cl  

; Examples
shr ax, 8  
sar rcx, 32  
shr rax, cl  
sar qword [qNum], cl
```
![[shr-instruction.png]]


## sar instruction
- The arithmetic right shift moves the bits the number of specified places to the right but treats the operand as a signed number which preserves the sign
- In layman's terms the MSB is maintained 
- The CF flag is loaded withe the last bit shifted out
- the immediate number is the number if places to shift
- the destination operand cannot be immediate 

``` nasm
; Syntax 
sar <dest>, <imm>  
sar <dest>, cl  

; Examples 
sar ax, 8  
sar rcx, 32  
sar rax, cl  
sar qword [qNum], cl
```
![[sar-diagram.png]]