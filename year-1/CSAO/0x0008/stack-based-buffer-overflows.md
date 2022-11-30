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
#include <string.h>
#include <stdio.h


```
