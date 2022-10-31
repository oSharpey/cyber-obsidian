# Memory Categories
- 4 memory categories: [[registers|CPU Registers]] and cache, Physical memory, Solid State Memory, Virtual Memory
- As you go up the pyramid shown below the volatility increases 
	- CPU registers = most volatile
	- Virtual Memory = least volatile

![[memorycat.png]]


### Physical Memory (RAM)
- Stores code and data for the computer
- Contains an array of bytes, each byte labelled with a unique number called an address
- Stored in [[data-representation|little-endian]]

### [[registers]]
- Small amount of memory in the CPU
- Used to store values fetched from RAM and results of calculations

### Virtual Memory
- Used when there is no more space in RAM
- OS allocates some space on the hard disk (usually a separate partition) to store data for a process. 
- You can sometimes find remnants of work in this useful during digital forensics  
