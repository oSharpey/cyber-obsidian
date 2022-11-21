# Memory Management - Paging

## Paging
- Separates physical memory into page frames or pages 
- When you allocate memory you allocate it page by page (allocates memory in fixed size chunks)
- Avoids external fragmentation (no small holes in memory)
- Has internal fragmentation (partially filled pages)

### Advantages
- Abstracts the process' address space effectively 
- Simplifies free space management
	- allocates the required number of pages to the process
	- mapping between AS and pages maintained in page table

### Page Table
- Each process has its own page table
- The process' page table gets accessed when the process has time on the CPU after a context switch

### Page table entry
- Page table is an array of page table entries (PTE)
- Each row is called a page table entry and represents a virtual page (virutal page stores location of physical page)
- Page table frame based on CPU architecture 
- Each PTE contains a PFN (physical frame number) and a few other bits
	- Valid Bit
	- Protection Bit
	- Present Bit
	- Dirty Bit
	- Accessed Bit

### What Happens on Memory Access