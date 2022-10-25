# Hubs
- Like a switch but with no memory 
- Layer 2

![[hub-diagram.png]]

- Hubs broadcast out on all ports except the source port
- Useful for signal regeneration about 35 years ago
- Netkit uses hubs in `a[0] = LANX` type of commands

# Switches
- Like a hub but with memory
- Also called a bridge
- Switches start off effectively as a hub, but as network traffic goes through it the switch learns (caches MAC addresses)
![[switch-diagram-01.png]]

## 2 switches connected together
![[2-switch-diagram.png]]
![[two-switches-notes-01.png]]
![[two-switch-notes-02.png]]
![[two-switches-notes-03.png]]


# Constraints
## Hard
- No duplicates
	- 2 frames will get sent out at the same time
	- Switch will have to store one of the frames in the buffer
	- If the buffer is full the frames will get dropped, no copies will be made of dropped frames
- Frames will always end up in order

## Soft
- Bandwidth is cheap - Don't care how much traffic you are sending on a LAN
- Latency should be low 
	- Not waiting ages for a response 









