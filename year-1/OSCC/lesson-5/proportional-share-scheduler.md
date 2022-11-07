# The Proportional Share Scheduler

- Guarantees the each job obtains a certain percentage of CPU time
- Periodically hold a lottery to determine which process should get to run next
- Processes that should run more often should be given more chances to win the lottery
- Also known as 
	- Fair Share Scheduler
	- Lottery scheduling

## The lottery
- The scheduler issues a set number of tickets
- The total number of tickets composes the total CPU time
- Each ticket represents a CPU time slice
- The proportion of tickets that a process holds represents its share of the CPU time
- As you add more processes you can reduce the time slice for each ticket
- At every cycle:
	- The scheduler randomly selects a winning ticket
	- The process holding the winning ticket is given access to the CPU

## Advantages of Using Randomness
- Simplifies the overall algorithm 
	- No need for detailed accounting like with MLFQ
- No need to worry about many of the worst case scenarios with fairness issues
- Computationally light
	- As long as you have a fast pseudo-random number generator


## Working With Tickets
- Ticket transfer
	- A process can transfer some of its tickets to another process
	- Boosts another process to execution
- Ticket currency
	- A user can hold tickets and distribute them to its processes using a different value for its tickets
	- The user's 'currency' is converted to the global currency when tickets are sent to the scheduler
- Ticket inflation
	- A process can lower or increase the number of tickets it holds
	- Massive security problem so only works in trusted environments 


## Stride Scheduling
- Each process has 
	- *Stride* = inverse in proportion to the number of tickets it has, the more tickets the lower the stride
	- maintain a counter called a *pass* accumulating the strides used
- Algorithm
	- run the process with the lowest pass
	- *pass* += *stride*



