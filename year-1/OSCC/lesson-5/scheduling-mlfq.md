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

### Problems 
- High priority queue can monopolise time on the CPU


## MLFQ Algorithm Rules
**Rule 1**:
If Priority(A) > Priority(B), A runs and B doesn't

**Rule 2**
If Priority(A) = Priority(B), A & B run in round robin

**Rule 3**:
When a job enters the system, it is placed at the highest priority (assumes all processes are interactive)

**Rule 4a**:
If a job uses up the entire CPU time slice while running it gets moved down 1 priority level

**Rule 4b**:
If a job gives up the CPU before it uses an entire time slice, it stays on the same priority level


![[images/mlfq-image-1.png]]
^ Showing how MLFQ works using the rules above with one long (batch) job and a long job & short job (from scheduling cont. PP by Dr Christo Panchev)

![[images/mlfq-image-2.png]]
^ Showing how MLFQ works using the rules above with one long (batch) job and an interactive I/O heavy job (from scheduling cont. PP by Dr Christo Panchev)

## Issues With These 4 Rules
- Batch process starvation
- You are able to attack/game the scheduling algorithm (monopolise the CPU)
- What happens if a process changes its behaviour (from batch to interactive)
	- No rule to increase the priority of a process

## Possible MLFQ Fixes

### Priority Boost
**Rule 5**:
After some time period decided by the kernel programmer or user, move all the jobs to the highest prior
