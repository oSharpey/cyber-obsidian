# System Calls 

## What is a system call?
-  System call --> A function that runs in kernel mode that has access to system resources 
	- User process can access system resources through system calls
	- Allows the OS to control access to the resources
	- Implemented through trap instructions and the **trap table**

## How the system call works
1) A process issues a system call to the OS
2) An interrupt is raised 
3) Looks into trap table and finds the address of the interrupt
4) CPU starts executing the interrupt
5) The CPU switches to kernel mode
6) \<Do the job\>
7) CPU switches back to user mode
8) Return to the code

- The kernel still needs to maintain a stack however it does not trust the user mode stack
- To fix this system calls run on a separate **kernel stack**



 

