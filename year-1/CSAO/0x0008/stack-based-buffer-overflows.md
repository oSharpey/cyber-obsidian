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


