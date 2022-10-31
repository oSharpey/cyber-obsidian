# Processes

### What is a process
- An instance of an application
- an abstraction of a running program
- Instruction + data + register values that are loaded into memory


### Process creation
- System initialisation --> boot up system, the kernel loads up (kernel is the root where all processes originate)
- System call by running a process --> [[the-kernel|kernel]] can load up GUI, file explorer, etc. (runs in a hierarchy)
- The user can request to start a new process


### Process Termination 
- Normal termination --> an explicit call to exit, everything finished normally
- Abnormal Termination
	- Error exit --> an error caused by the process, often due to a program bug (illegal instruction, dividing by zero)
	- Fatal Error --> process discovers a fatal error (open a non existent file for example
	- Killed by another process --> process executes a system call telling the OS to kill another process, `kill` command in UNIX or `TerminateProcess` in windows 


### Process states
- 4 main process states --> running, ready, blocked, terminated 
- Running 
	- Process's instructions are currently being executed by the CPU
- Ready 
	- Temporarily stopped to let another process run
- Blocked 
	- Process need to wait for an external event before it can run (I/O event, receive data, complete submission of data)
	


### Process Control Block#
- Process management 
	- [[scheduling|Scheduling]]
- Memory management
	- Works with logical addresses
	- OS maps logical addresses to physical addresses
	- CPU sees memory as a contiguous block
- File management 
	- Network sockets
	- Access control for process (UserID, Group ID)


**3 main parts of a process - Instructions, static and runtime data, process control block**