# The Proportional Share Scheduler

- Guarantees the each job obtains a certain percentage of CPU time
- Periodically hold a lottery to determine which process should get to run next
- Processes that should run more often should be given more chances to win the lottery
- Also known as 
	- Fair Share Scheduler
	- Lottery scheduling

## The lottery
- The scheduler issues a set number of tick