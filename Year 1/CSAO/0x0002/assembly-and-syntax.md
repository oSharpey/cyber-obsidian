# Assembly and Syntax Fundamentals

## Different Assemblers Use Different Syntax
### **There are 3 main assemblers used today**
- The GNU Assembler [GAS]
	- Uses AT&T Syntax
- The Microsoft Macro Assembler [MASM]
	- Uses Intel Syntax
- The Netwide Assembler [NASM]

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
```
extrn ExitProcess : proc

.data
	sum QWORD 0

.code
	_main PROC

```


