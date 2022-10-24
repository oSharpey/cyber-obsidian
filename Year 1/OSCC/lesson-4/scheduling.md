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
- Hardware timer interrupt --> 2 policies:
	- Non-preemptive --> let the process run until it finishes
	- Preemptive --> can interrupt the current running process


# Scheduling Policies
- All depends on the environments 
	- Batch Systems
		- Mainframes
		- Submits jobs to a queue
		- Non-preemptive
	- Interactive systems (general purpose OS)
		- Interacts with the user
		- Cannot afford to run one job at a time
		- Preemptive
	- Real time systems (IOT sensors)
		- Needs to process data in real time
		- Usually preemptive


#  Scheduling Metrics
- All systems
	- Fairness - Give each process a fair amount of time on the CPU
	- Policy enforcement 
	- Balance - Keep all parts of the system busy
- **Batch Systems**
	- Throughput - maximise the number of jobs per hour
	- Turnaround time - "minimise time between submission and termination" 
	- CPU utilisation - Keep the CPU busy at all times
- **Interactive systems**
	- Response time - minimise time between submission and first run
	- Proportionality - meet users expectations, some processes may have higher priority to run
	- *There has to be a compromise with how busy the hardware is and the interactivity of the system*
- **Real Time systems**
	- Meeting deadlines - avoids data loss
	- Predictability - avoid losing quality in multimedia systems (streaming etc)




