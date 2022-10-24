# Non-preemptive 
- Process is allowed to run until the job finishes
- Other processes will join the queue
- A process can only be removed from the CPU if it has been blocked 

## FIFO - First in, first out

**The first submitted is the first to be executed**

### All processes with the same execution time

![[fifo-optimal.png]]
The image above shows the optimal conditions for FIFO
The turnaround times for each process are as follows:
- A: 10s
- B: 20s
- C: 30s
This gives an average turnaround time is 20s

**What if the conditions aren't optimal?**

### One process is longer
![[fifo-one-longer.png]]
The image above shows some more realistic  for FIFO
The turnaround times for each process are as follows:
- A: 100s
- B: 110s
- C: 120s
This gives an average turnaround time is 110s

This shows the disadvantages of FIFO, it is very simple however it cannot be optimised leading to long turnaround times

## SJF - Shortest Job First

### All processed submitted at the same time
![[sjf-optimal.png]]
The turnaround times for each process are as follows:
- A: 120s
- B: 10s
- C: 20s
This gives an average turnaround time is 50s

### One job arrives late
![[sjf-late-arrival.png]]


The turnaround times for each process are as follows:
- A: 100s
- B: 110s
- C: 120s
This gives an average turnaround time is 110s

**A late job causes SJF to have the same average turnaround time as FIFO**

---

# Preemptive
- A process can be taken off the CPU by the scheduler
- Turnaround time does not matter as much anymore, response time is more important 

## STCF - Shortest Time to Complete First
![[stcf.png]]

Turnaround times
- A: 120s
- B: 10s
- C: 20s
- Avg: 50s
Response Times
- A: 0s
- B: 0s
- C: 10s
- Avg: 3.3s


## Round Robbin

![[round-robbin.png]]
Response Times
- A: 0s
- B: 1s
- C: 2s
- Avg: 1s


## The problem with I/O

### No overlapping
- The scheduler in the image below does no allow process B to have time on the CPU once process A is blocked by I/O. 
- Causes the throughput to be very low 


