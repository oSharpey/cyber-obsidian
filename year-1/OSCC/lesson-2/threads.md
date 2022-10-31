# Threads

### What is a thread
- A thread of execution of the process instructions
	- Processes startup as single threads

- Similar to a process but not exactly the same
- A line of instructions being sent to the CPU
- Used with lots of I/O, eg. in user interfaces
- Can split a process into chunks

- It allows you to implement concurrency within a process (Multi-threading)
	- Often in IO handling tasks 
	- Implement parallel tasks
- A 'lightweight process'
	- Faster to create/destroy compared to processes
- All threads within a process share the same address space


### [[processes|Process]] vs Threads
- Process 
	- Single instance of an application with a unique address space
	- Access global variables
	- Open files
	- Create child processes
- Threads
	- All share the same address space
	- Much faster to create and destroy compared to processes
	- All threads share the same address space --> for example if all users are connecting to a web server using a new thread for each user, every user can access the others address space
	- Due to the reason above processes are used in some applications over threads for security reasons, even tho they are slower


### Execution Stack
- Each thread has its own execution stack
	- A frame for each procedure called 
		- Local variables (incl arguments)
		- Return addresses 
- Eg. function funA() calls funB(arg=3)

![[execution-stack.png]]

