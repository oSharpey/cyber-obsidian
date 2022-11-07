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

## Integer Addition

