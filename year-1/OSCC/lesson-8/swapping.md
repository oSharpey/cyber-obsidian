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
- configured with 2 thresholds
	- Low Watermark (LW) - start swapping
	- High Watermark (HW) - no need to swap
- Swap multiple pages at once
	- Optimises disk I/O

## Static Pages
- Pages that contain entirely executable code, do not need to be swapped
- You can safely reuse the memory page, and load the code from the original file later

## The Present Bit
- All TLB hit pages are in the memory (1)
- On a TLB miss (0)
	- The hardware looks at the page table entry
	- PTE has a bit identifying if the page is in memory
		- if it is present - cache in TLB and use
		- If not present - Page Fault (page you are looking for not in physical memory, need to swap back into memory)


## Page Fault Handling
- Manages by the OS page-fault handler
	- PTE contains the location of the page in the swap space
	- OS calls the file system to load the page
	- OS updates the PTE - PFN to point to the page in memory
	- Return to hardware to retry address resolution
- OS might have to swap out another page to free up memory
- The affected process is in a blocked s