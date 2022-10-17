# Context Switching

## What is Context Switching
- At some point we will want to take a process off the CPU and switch to a second process
- Context switching is used to achieve this

## How it works
- The CPU saves the registers of process A to the kernel stack
- The CPU changed to kernel mode
- The switch() trap instruction is called
- Saves the contents of A's registers to the process table
- Loads process B's registers from the process table
- Switches to B's kernel stack
- Restores B's registers from the kernel stack
- Switch the CPU to user mode
- Process B is run. 
