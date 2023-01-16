# Persistence
## Input/Output Devices
- I/O devices connect to the CPU and memory via a bus
	- High Speed bus: PCIe, PCI
	- Other: SCSI, USB, SATA
- Point of connection: Port

## Simple Controller
- Block Devices store a set of numbered blocks (eg. disks)
- Character devices produce/consume a stream of bytes (eg. keyboard)
- Devices expose an interface of memory registers
	- Current status of device
	- Command to execute
	- Data to transfer
- The internals of device are usually hidden

## Reading/Writing registers
- Explicit I/O instructions
	- Eg. on x86, in and out instructions can be used to read and write specific registers on a device
	- Always in kernel land - no user has access to peripherals/devices
- Mapped memory I/O
	- Device makes registers appear like memory locations
	- OS simply reads and writes from memory
	- Memory hardware routes accesses to these special memory addresses to devices

## Polling
- Polling status to see if device is ready - suspends CPU - wastes CPU cycles

## Interrupts
- Instead of polling, OS can put process to sleep and do a context switch to another process
- Synchronised by using interrupt
- When disk has finished the job it will send an interrupt 

## Interrupt Handler
- Interrupt switches process to kernel mode
- Interrupt Descriptor Table (IDT) stores pointers to interrupt handlers
	- IRQ number identifies the interrupt handler to run for a device
- Interrupt handler acts upon device notification, unblocks the process waiting for I/O and starts the next I/O request
- Handling interrupts imposes kernel mode transition overheads (context switches use resources)

## Direct Memory Access
- CPU cycles are wasted in copying data to/from a device
- Instead, a special piece of hardware called a DMA engine copies from main memory to the device
	- CPU will write control signals into the memory, DMA will read these from the memory
	- CPU will now start another process, DMA takes these control signals and sends them to the device
	- Read: interrupt raised after DMA completes
	- Write: disk starts writing after DMA completes

## Device Drivers
- Device Independence (eg. reading and writing files without worrying precisely about what device it is)
- Uniform naming
- Error handling
- Synchronus (blocking) vs asynchronous (interupt-driven) transfers
- Buffering
- Shared vs Dedicated drivers

## Hard Disk Internals
- Interface: a set of (512-bytes on old, 4KiB on new disks) blocks (sectors), that can be read or written atomically
- Internals: one or more platters, connected by a spindle spinning at either 5400rpm, 7200rpm or 10000rpm
- Each platter has a disk head and arm
- A platter is divided into sectors
![[disk-sector.png]]
- Smallest writable bit on a disk is a sector - sector size is defined by the formatting of the disk (vFAT, NTFS)

## Disk terminology
- Data is addressed at sector level and stored in sector sized chunks
- Data is addressed using LBA at the OS/driver level and converted into something else (cylinder-head-sector (CHS)) by the disk firmware
- A cluster is a fixed size of sectors
	- Disk space is allocated in whole clusters
	- 128 sectors per cluster for example
- Slack Space: wasted space on the disk when the file does not need all the space that has been allocated to it
	- Can be used to hide stuff as OS will not touch it as long as the file is there
	- Pick up slack space of a file that will never be deleted
	- Can only access when doing raw disk read

## Accessing a sector
- Suppose disk head is at 30, we need to access 11
- Seek to the correct track and wait for the disk to rotate
- This is quite slow

## Time taken for I/O
- Time taken to read/write a block (random reads) consists of:
	- Seek time to get to the right track (few ms)
	- Rotational latency for disk to spin to correct sector (few ms)
	- Transfer time to read sector (tens microsec)
- Sequential Reads: choose a track and read all the sectors
- Usually sequential reads are faster than random reads