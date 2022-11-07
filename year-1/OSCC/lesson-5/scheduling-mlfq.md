# Scheduling Continued 
## Multi-level Feedback Queue (MLFQ)
- How can we build a scheduler without any inferred knowledge about the processes
	- Learn from the past
	- Phases of predictable behaviour 
	- Basically figure out how long a similar process will run in the future
- Trying to 
	- Minimise turnaround time
	- Minimise response time


## MLFQ Basics
### Multiple queues
- Each new process is put in a queue
- Queues are ranked my priority 
- Each queue has a different priority level
- Processes in a higher priority queues are ran first
- Processes in the same priority queue run in a round robin 

**Priority for a process can vary** 

### Processes are put into a queue based on:
- Predefined priority 
	- Start at highest priority and punish certain processes
- Observed behaviour 
	- Processes with lots of I/O gets higher priority 
	- Processes that use the CPU the most get lower priority 
