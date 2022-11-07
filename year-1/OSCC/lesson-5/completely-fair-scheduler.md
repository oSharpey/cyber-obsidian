# The Completely Fair Scheduler (CFS)
- Used in linux
- A computationally optimised implementation of the Proportional Share Scheduler
	- Minimise time spent on the scheduling decisions
	- Optimise data structures used
- Aims to fairly divide a CPU evenly among all competing processes
- Multi queue approach in multi-core systems
- Uses binary tress instead of lists
- Fairness *does not* mean equal
- Fairness is proportional to all other processes
- Need to minimise switching of processes between cores

## The CFC Algorithm
- Maintain per-process virtual runtime (*vruntime*)  - the total time spent on the CPU
- Pick the process with the lowest *vruntime*
- CPU burst time is given by *sched_latency* divided by the number of ready processes 
	- Typical value for *sched_latency* is 48ms
- CPU burst time cannot be less than the minimum granularity 
	- Typical value for the minimum granularity is 6ms
- Uses a balanced tree
	- Binary tree
	- Maximum difference in depth is 1


## Process priority with CFS
- Uses the Linux *nice* levels
	- Default values from -20 to +19
	- A lower value means a higher priority 
- Affects the length of the CPU time slice (CPU burst) given to the process


## Data Structures
- Red-Black trees
	- A balanced tree containing processes in ready state
	- Searching time becomes logarithmic
- Built and maintained based on the processes' *vruntime*


## Dealing with processes in Blocked state
- Problem:
	- A process which is in a blocked state for a long time will have a very low *vruntime* and monopolise the CPU when they return
- Solution:
	- Reset the *vruntime* of a returning process to the lowest value currently in the tree