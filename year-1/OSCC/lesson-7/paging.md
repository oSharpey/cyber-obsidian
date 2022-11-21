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
- how does MMU know where page table is?
	- Special CPU register that stores the start of the page table

### Address Translation in Hardware
- The MSB of the virtual address gives the virtual page number

### TLB
- Every time the CPU wants to read memory the MMU has to look up multiple things from memory 
	- This is computationally expensive and slow
- TLB solved this issue
- TLB will cache recent translations of virtual to physical page mappings
- So now when translating address you dont have to go to RAM multiple times you can look up the cache
- When requesting memory
	- If TLB is it, PA can be directly used
	- If TLB miss, then MMU performs additional memory access to "walk" the page table, then the new mapping will be stored in TLB
- TLB misses are expensive 