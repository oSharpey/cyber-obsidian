## Understanding Buffer Overflows
- Happens when data written to a buffer is larger than the size of the buffer
	- Due to insufficient bounds check, it overflows and overwrites adjacent memory locations
- The result
	- Allows an attacker to execute arbitrary commands in the machine
	- Take over system or escalate privileges
	- Some work locally and others across the network
- Root Cause
	- Program code which does not perform proper bounds checking

**Example**
```c
// day7_ex1.c

#include <string.h>
#include <stdio.h>

int main(int argc, char** argv)
{
	argv[1] = (char*)"ABCDEFGHIJKLMNOPQRSTUVWXYZ";
	char buffer[8];
	strcpy(buffer, argv[1]);

	return 0;
}
```

- The buffer is 8 bytes long
- The code uses the function `strcpy`
- The code tries to copy more than the buffer can handle via the `strcpy` function
	- `argv[1]` contains 26 characters, while the buffer can only handle 8
- When the program runs, the exceeding data will overwrite some adjacent memory addresses

### strcpy() Function
```c
char* strcpy(char* destination, const char* source)
```
- Copies string pointed by source (including null byte) to the destination
- The strcpy() returns the copied string


## Tutorial
```c
#include <string.h>
#include <stdio.h>

int main(int argc, char** argv)
{
	argv[1] = (char*)"ABCDEFGHIJKLMNOPQRSTUVWXYZ";
	char buffer[8];
	strcpy(buffer, argv[1]);

	return 0;
}
```

Compile the code turning off stack protection:
```sh
gcc -fno-stack-protector -z execstack day7_ex1.c -o day7_ex1
```


### The stack before strcpy()
```c
[------------------------------------stack-------------------------------------]
0000| 0x7fffffffdf50 --> 0x7fffffffe088 --> 0x7fffffffe4f5 ("/home/oscar/Documents/bsc-cyber/csao/0x0008/day7_ex1")
0008| 0x7fffffffdf58 --> 0x1f7fe5afe
0016| 0x7fffffffdf60 --> 0x0
0024| 0x7fffffffdf68 --> 0x7ffff7ffdab0 --> 0x7ffff7fc8000 --> 0x3010102464c457f
0032| 0x7fffffffdf70 --> 0x1
0040| 0x7fffffffdf78 --> 0x7ffff7ddb5b0 (<__libc_start_call_main+130>:	mov    edi,eax)
0048| 0x7fffffffdf80 --> 0x7fffffffe070 --> 0x7fffffffe078 --> 0x38 ('8')
0056| 0x7fffffffdf88 --> 0x401146 (<main>:	push   rbp)
[------------------------------------------------------------------------------]
```

### The stack after strcpy()
```c
[------------------------------------stack-------------------------------------]
0000| 0x7fffffffdf50 --> 0x7fffffffe088 --> 0x7fffffffe4f5 ("/home/oscar/Documents/bsc-cyber/csao/0x0008/day7_ex1")
0008| 0x7fffffffdf58 --> 0x1f7fe5afe
0016| 0x7fffffffdf60 --> 0x0
0024| 0x7fffffffdf68 ("ABCDEFGHIJKLMNOPQRSTUVWXYZ")
0032| 0x7fffffffdf70 ("IJKLMNOPQRSTUVWXYZ")
0040| 0x7fffffffdf78 ("QRSTUVWXYZ")
0048| 0x7fffffffdf80 --> 0x7fffff005a59
0056| 0x7fffffffdf88 --> 0x401146 (<main>:	push   rbp)
[------------------------------------------------------------------------------]
```

### The stack after leave instruction
```c
[------------------------------------stack-------------------------------------]
0000| 0x7fffffffdf78 ("QRSTUVWXYZ")
0008| 0x7fffffffdf80 --> 0x7fffff005a59
0016| 0x7fffffffdf88 --> 0x401146 (<main>:	push   rbp)
0024| 0x7fffffffdf90 --> 0x100400040
0032| 0x7fffffffdf98 --> 0x7fffffffe088 --> 0x7fffffffe4f5 ("/home/oscar/Documents/bsc-cyber/csao/0x0008/day7_ex1")
0040| 0x7fffffffdfa0 --> 0x7fffffffe088 --> 0x7fffffffe4f5 ("/home/oscar/Documents/bsc-cyber/csao/0x0008/day7_ex1")
0048| 0x7fffffffdfa8 --> 0x4b31a3483fb4b46f
0056| 0x7fffffffdfb0 --> 0x0
[------------------------------------------------------------------------------]
```

- The return address has been overwritten by some of the data used in the strcpy() function
- Causes a seg fault


### Replace strcpy() with strncpy()
```c
#include <string.h>
#include <stdio.h>

int main(int argc, char** argv)
{
	argv[1] = (char*)"ABCDEFGHIJKLMNOPQRSTUVWXYZ";
	char buffer[8];
	strncpy(buffer, argv[1]);

	return 0;
}
```