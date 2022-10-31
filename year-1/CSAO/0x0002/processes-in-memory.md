# Loading and Executing a Binary --> Memory Layout

- When you run a process it is organised in memory as shown in the picture below 
	- **Code**
		- Stores the instructions to execute
		- Read only as we do not need to alter the code once compiled 
		- In the .text section
		
	- **Data**
		- Stores the static and global variables
		- Read only or Read-Write
		- Sections: .data(rw) .bss(rw) .rodata(r)
		- .data stores initialised data
		- .bss stores uninitialised data
		
	- **Heap**
		- Dynamically allocated memory
		
	- **Stack**
		- Contains local variables and function arguments
		- Controls the program flow


![[process-mem-layout.png]]