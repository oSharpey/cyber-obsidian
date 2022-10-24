# What is CPU Scheduling
- When a context switch happens, which process out of the pool of ready processes will be given time on the CPU (run the process)
- The scheduler schedules the CPU requests (bursts, time shares) of processes
	- Burst/time-share --> the CPU times used by a process in a continuous stretch
	- If a process comes back after being blocked by I/O it counts as a new burst
	- There is no guarantee which process will have time on the CPU next


# Why do you need to schedule
- Forking processes --> the parent and child are both in a ready state, need to decide which one gets ran first
- If a process has terminated --> next process to run needs to be decided 
- If a process has been blocked by I/O



