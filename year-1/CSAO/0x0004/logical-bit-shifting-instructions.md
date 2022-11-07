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
```
Syntax: or <dest>, <src>  
Examples: or ax, bx  
or rcx, rdx  
or eax, dword [dNum]  
or qword [qNum], rdx
```