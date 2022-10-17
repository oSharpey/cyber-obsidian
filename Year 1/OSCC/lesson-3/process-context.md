
# Process Context

## What is process context?
- Used in [[context-switching|context switching]]
- Saved when a process is taken off the CPU
- Essentially a data structure that contains everything a process needs to function (everything currently stored in the CPU)
	- General registers
	- Program counter
	- Stack pointer
- Loaded back when the process gets sent back to the CPU