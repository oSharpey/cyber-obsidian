# Direct Execution


## What is direct execution
- Running the program directly on the CPU


## Running a process with direct execution
 
**(OS)**
- OS will create a new entry in the process list
- OS will allocate memory to store instructions, static data etc.
- The program will be loaded into main memory
- The stack will be set up --> one entry for each function called
- The bottom of the stack (first entry) will contain the main() function
- Process will now be in 'ready' state ready to be executed by the CPU
- OS will clear the registers 
- Execute call to main() function

**(Process)**
- Run code
- Execute a return from the main() function

**(OS)**
- OS will free the memory used by the process
- Process gets removed from the process list


## User mode & Kernel mode
- User mode is limited to the type of instructions it can run
- Kernel mode has no limit on the type of instructions that can run
- Everything the OS does is in kernel mode
- When handing over control to the process the CPU switches to user mode


## There are problems with direct execution

### Problem 1
- Processes need to be able use additional resources not available in user mode (accessing I/O, changing registers)
	- It is not a good idea to give the process direct access to kernel mode, this breaks [[cia|CIA]]

- **Solution: Introduce [[system-calls|system calls]]**

### Problem 2: [[context-switching|Switching between processes]]
- We need to ensure a mechanism to switch between processes
- Ensure the OS has time to run on the CPU
- **Solution: Use [[system-calls|system calls]]**
	- Using a hardware timer: *timer interrupts*
		- Hardware time generates interrupts
		- Interrupt handler (OS) is called similar to a system call (calls the kernel from the trap table allowing the kernel to have time on the CPU)