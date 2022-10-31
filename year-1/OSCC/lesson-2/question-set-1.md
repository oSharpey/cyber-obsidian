## Question Set 1


### Question 1
- Both processes run on CPU, 100% utilisation, no parallelisation

### Question 2
- 4 cycles where CPU is busy on first process, 1 cycle busy on second process. 4 cycles waiting on I/O to finish

### Question 3
- Runs IO process, whilst waiting for IO to finish CPU Runs process 2. Running IO first cuts down total time by 4 cycles (from 10 to 6)
- Introduces parallelisation 

### Question 4
- Unlike the question above when the IO is issued and waiting the CPU does not run the second process in parallel, instead waiting for the IO to finish and then the second process runs

### Question 5
- Performs standard behaviour, runs the second process whilst the IO is waiting to finish. 

### Question 6
- Does not effectively use system resources, process 1 runs whist process 0 IO is waiting as to not be idle. However it does not run the 2 later IO processes concurrently with process 2 and 3

### Question 7
- Does effectively use system resources, process 1 runs whist process 0 IO is waiting as to not be idle. Unlike number 6 it does run the second 2 IO processes in parallel with process 2 and 3



[[processes]] [[threads]]


IO operations considered separately as they are much slower than regular processes. We don't have full control over what happens with IO

