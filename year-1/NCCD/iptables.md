# iptables

## What is iptables
- manage Linux firewalls
- manage packet filtering ans NAT rules
- Contains 4 built in tables
- each table contains a chain
- each chain can have one or more rules

### Tables
- Filter table
- NAT table
- Mangle table
- Raw table

### Filter Table
- Default table
	- INPUT chain: Incoming to firewall. 
		- Packet not generated on device coming from other source and the device is receiving the packer
	- OUTPUT chain: Outgoing from firewall. 
		- Device generated the packet and is send through one of the interfaces
	- FORWARD Chain: packet for other network card or routed packets

### NAT Table
- Handle Nat Related rules
- Output chain: NAT for locally generated packers

### Mangle table
- specialised packet alteration like QoS bits and TCP header
### Raw table
- Used for O