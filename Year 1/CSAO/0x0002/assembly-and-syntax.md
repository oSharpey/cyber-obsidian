# Assembly and Syntax Fundamentals

## Different Assemblers Use Different Syntax
### **There are 3 main assemblers used today**
- The GNU Assembler [GAS]
	- Uses AT&T Syntax
- The Microsoft Macro Assembler [MASM]
	- Uses Intel Syntax
- The Netwide Assembler [NASM]


### **AT&T vs Intel**

#### AT&T
- Prefixes registers with *%* and constants with *$*
- Different order of operands --> `mov $0x6, %edi`
	- The source is *$0x6* and the destination is *%edi*

#### intel
- There are no prefixed on the registers and constants 
- The order of the operands is opposite of AT&T --> `mov edi, 0x6`
	- Source is *0x6* and the destination is *edi* 

### **Syntax**
#### GAS
``` nasm
.data
	sum: .quad 0

.text
	.global _main
		_main:
			mov $25, %rax
			mov $50, %rbx
			add %rbx, %rax
			mov %rax, sum(%rip)

			mov $0x200001, %rax
			xor %rdi, %rdi
			syscall

.end
```

#### MASM
``` nasm
extrn ExitProcess : proc

.data
	sum QWORD 0

.code
	_main PROC
		mov rax, 25
		mov rax, 50

		xor rcx, rcx
		call ExitProcess
	_main ENDP

END
```

#### NASM

```nasm
global _start

section .text
	_start:

		;Syscall to write() function
		mov rax, 1  ; 1 indicated write function
		mov rdi, 1  ; 1st arg in write(), file descriptor, 1 = stdout
		mov rsi, hello_world   ; 2nd arg, buffer pointer, points to start address of hello_world
		mov rdx, length  ; 3rd arg, count
		syscall


		;Syscall to exit function needed as to not cause seg fault
		mov rax, 60  ; 60 indicated exit function 
		mov rdi, 0  ; insure instruction pointer is empty, could also use xor rdi, rdi
		syscall
		

section .data
	hello_world: db 'hello world', 0xa
	length: equ $ - hello_world
```

