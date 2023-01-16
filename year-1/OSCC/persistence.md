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

## Reading/Writing registes