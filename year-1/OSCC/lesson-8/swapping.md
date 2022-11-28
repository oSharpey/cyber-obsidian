# Swapping

## Swap Space
- A reserved space in the secondary storage (HDD/SSD)
	- Can be implemented in different ways, Linux generally use swap partitions and Windows & macOS uses swap files
	- Size of the swap space is a critical variable
	- Size is pre-configured by OS and reserved during instillation 

## Swapping
- If the system runs of physical memory
	- It can transfer VM pages which are not currently used onto the swap space
	- Use the freed physical memory space for currently active processes

## When to swap
- OS will run a daemon in the background to make sure there is some physical memory availaible
- configued with 2 thresholds
	- Low Watermark (LW) - start swapping
	- High Watermark (HW) - no need to swap