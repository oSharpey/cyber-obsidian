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
- Polling status to see if device is ready - suspends cpu - wastes cpu cycles

## Interrupts
- Instead of polling, OS can put process to sleep and do a context switch to another process
- Synchronised by using interrupt
- When disk has finished the job it will send an interrupt 

## Interrupt Handler
- Interrupt switches process to kernel mode
- Interrupt Descriptor Table (IDT) stores pointers to interrupt handlers
	- IRQ number identifies the interrupt handler to run for a device
- Interupt handler acts upon device notification, unblocks the process waiting for I/O and starts the next I/O request
- Handling interupts imposes kernel mode transistion overheads (context switches use reso)
