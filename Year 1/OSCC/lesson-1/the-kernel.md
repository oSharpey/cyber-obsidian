# The Kernel

### What is the kernel and what does it do?
- The core part of the [[what-is-an-os|OS]] 
- Acts as the main layer between the underlying hardware and OS
- Manages memory allocation and CPU Scheduling (access to the CPU's resources) 
	- *These are both managed via **virtualisation**


### Running an Application
Hard Disk (Application) ---> Memory (Process) ---> CPU (Instruction)


**Application vs Process vs Instruction**
- *Application*
	- The actual software (code/executable) that is stored on the hard disk
- *[[processes|Process]]*
	- An application that has become live and is now stored in memory
	- "*An instance of an application*"
- *Instruction*
	- Single commands and data that is executed by the CPU


***In [[2-types-of-os|microkernels]] all hardware I/O is controlled by device drivers (a kernel module)***